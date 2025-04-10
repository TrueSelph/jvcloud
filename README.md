# JVCloud - Helm Chart for JIVAS Deployments

[![Helm Version](https://img.shields.io/badge/helm-v3-blue)](https://helm.sh)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![GitHub Release](https://img.shields.io/github/v/release/TrueSelph/jvcloud)](https://github.com/TrueSelph/jvcloud/releases)

A Helm chart for easily deploying JIVAS application instances and related services in Kubernetes clusters. This chart provides complete configuration for JIVAS core application, MongoDB database, and optional services like Typesense and WPPConnect.

## 📋 Features

- **Full JIVAS Deployment**: Deploy the main JIVAS application with proper configuration
- **Integrated MongoDB**: Automatically configured MongoDB replica set
- **Optional Services**: Enable/disable additional services (Typesense, WPPConnect)
- **AWS Integration**: Built-in AWS ALB ingress configuration with certificate support
- **Horizontal Scaling**: Configurable autoscaling for the JIVAS application
- **Secure by Default**: Automatic secret generation and management
- **Flexible Storage**: Support for both local and S3 storage

## 📝 Prerequisites

Before installing this chart, ensure you have:

1. **Kubernetes Cluster**: A working Kubernetes cluster (v1.19+)
2. **Helm**: Helm 3 installed on your workstation
3. **Domain Name**: A domain name for your JIVAS instances
4. **AWS Resources** (if using AWS):
   - ACM Certificate for HTTPS
   - AWS Load Balancer Controller installed in your cluster
   - IAM permissions for ALB creation
5. **Storage Classes**: Storage classes available in your cluster (default: `gp2`)

## 🚀 Installation

### Quick Start

```bash
# Add the TrueSelph Helm repository
helm repo add trueselph https://trueselph.github.io/jvcloud
helm repo update

# Install the chart with a release name and custom domain
helm install my-jivas trueselph/jvcloud \
  --namespace jivas-demo \
  --create-namespace \
  --set domain.name=example.com \
  --set aws.certificateArn=arn:aws:acm:region:account:certificate/id
```

### Installation from Source

Clone the repository and install directly:

```bash
git clone https://github.com/TrueSelph/jvcloud.git
cd jvcloud

helm install my-jivas ./jvcloud/helm/jvcloud \
  --namespace jivas-demo \
  --create-namespace \
  --set domain.name=example.com \
  --set aws.certificateArn=arn:aws:acm:region:account:certificate/id
```

## ⚙️ Configuration

### Core Settings

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `domain.name` | Domain for your JIVAS deployment | `""` | Yes |
| `image.repository` | JIVAS Docker image repository | `trueselph/jivas` | No |
| `image.tag` | JIVAS Docker image tag | `{{ .Values.jivas.version }}` | No |
| `image.pullPolicy` | Kubernetes image pull policy | `IfNotPresent` | No |
| `jivas.version` | JIVAS version to deploy | `latest` | No |

### MongoDB Configuration

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `mongodb.databaseName` | Name of the MongoDB database | `jivas` | No |
| `mongodb.replicas` | Number of MongoDB replicas | `2` | No |
| `mongodb.storage` | MongoDB storage size | `10Gi` | No |
| `mongodb.storageClassName` | Storage class for MongoDB | `gp2` | No |

### JIVAS Application Settings

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `jivas.environment` | Environment for JIVAS (production/development) | `production` | No |
| `jivas.config.actionsRootPath` | Path for actions | `actions` | No |
| `jivas.config.descriptorRootPath` | Path for descriptors | `.jvdata` | No |
| `jivas.config.filesRootPath` | Path for files | `.files` | No |
| `jivas.config.fileInterface` | File storage interface (`local` or `s3`) | `local` | No |
| `jivas.secrets.password` | Password for JIVAS (random if empty) | `""` | No |
| `jivas.secrets.webhookSecretKey` | Secret key for webhooks (random if empty) | `""` | No |
| `jivas.secrets.tokenSecret` | Secret for JWT tokens (random if empty) | `""` | No |

### S3 Configuration (When fileInterface = "s3")

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `jivas.config.s3.bucketName` | S3 bucket name | `""` | When using S3 |
| `jivas.config.s3.regionName` | S3 bucket region | `""` | When using S3 |
| `jivas.config.s3.accessKeyId` | AWS access key ID | `""` | When using S3 |
| `jivas.config.s3.secretAccessKey` | AWS secret access key | `""` | When using S3 |

### AWS Settings

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `aws.certificateArn` | ARN of the AWS ACM certificate | `""` | Yes for HTTPS |
| `aws.alb.group.enabled` | Enable ALB grouping | `true` | No |
| `aws.alb.group.nameFormat` | ALB group name format | `jivas-services` | No |
| `aws.dns.enabled` | Enable external-dns integration | `true` | No |
| `aws.dns.hostnameFormat` | Format for hostname annotation | `{{ .Release.Namespace }}.{{ .Values.domain.name }}` | No |

### Ingress Configuration

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `ingress.enabled` | Enable ingress resources | `true` | No |
| `ingress.className` | Ingress class name | `alb` | No |
| `ingress.services.jivas.enabled` | Enable main JIVAS ingress | `true` | No |
| `ingress.services.jivas.hostFormat` | Host format for JIVAS API | `{{ .Release.Namespace }}-jivas.{{ .Values.domain.name }}` | No |
| `ingress.services.jvclient.enabled` | Enable JVClient ingress | `true` | No |
| `ingress.services.jvclient.hostFormat` | Host format for JVClient | `{{ .Release.Namespace }}-jvclient.{{ .Values.domain.name }}` | No |
| `ingress.services.jvstudio.enabled` | Enable JVStudio ingress | `true` | No |
| `ingress.services.jvstudio.hostFormat` | Host format for JVStudio | `{{ .Release.Namespace }}-jvstudio.{{ .Values.domain.name }}` | No |

### Resource Requirements

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `jivas.resources.requests.memory` | Memory request for JIVAS | `512Mi` | No |
| `jivas.resources.requests.cpu` | CPU request for JIVAS | `250m` | No |
| `jivas.resources.limits.memory` | Memory limit for JIVAS | `1Gi` | No |
| `jivas.resources.limits.cpu` | CPU limit for JIVAS | `500m` | No |

### Optional Services

#### Typesense

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `optionalServices.typesense.enabled` | Enable Typesense service | `false` | No |
| `optionalServices.typesense.port` | Typesense port | `8108` | No |
| `optionalServices.typesense.protocol` | Typesense protocol | `http` | No |
| `optionalServices.typesense.connectionTimeoutSeconds` | Connection timeout | `5` | No |
| `optionalServices.typesense.storage` | Storage size for Typesense | `5Gi` | No |
| `optionalServices.typesense.storageClassName` | Storage class for Typesense | `gp2` | No |

#### WPPConnect

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `optionalServices.wppconnect.enabled` | Enable WPPConnect service | `false` | No |
| `optionalServices.wppconnect.image` | WPPConnect image | `registry.v75inc.dev/wppconnect/wppconnect_prod:latest` | No |
| `optionalServices.wppconnect.storage.tokens` | Storage size for tokens | `0.2Gi` | No |
| `optionalServices.wppconnect.storage.userData` | Storage size for user data | `1Gi` | No |
| `optionalServices.wppconnect.storageClassName` | Storage class for WPPConnect | `gp2` | No |
| `optionalServices.wppconnect.ingress.enabled` | Enable WPPConnect ingress | `true` | No |
| `optionalServices.wppconnect.ingress.hostFormat` | Host format for WPPConnect | `{{ .Release.Namespace }}-wppconnect.{{ .Values.domain.name }}` | No |
| `optionalServices.wppconnect.secretKey` | Secret key for WPPConnect (random if empty) | `""` | No |

### Autoscaling Configuration

| Parameter | Description | Default | Required |
|-----------|-------------|---------|----------|
| `autoscaling.enabled` | Enable autoscaling | `true` | No |
| `autoscaling.minReplicas` | Minimum number of replicas | `1` | No |
| `autoscaling.maxReplicas` | Maximum number of replicas | `5` | No |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization | `70` | No |
| `autoscaling.targetMemoryUtilizationPercentage` | Target memory utilization | Not set | No |

## 🔍 Usage Examples

### Basic Installation

```bash
helm install my-jivas trueselph/jvcloud \
  --namespace jivas-demo \
  --create-namespace \
  --set domain.name=example.com \
  --set aws.certificateArn=arn:aws:acm:region:account:certificate/id
```

### Enabling Optional Services

```bash
helm install my-jivas trueselph/jvcloud \
  --namespace jivas-demo \
  --create-namespace \
  --set domain.name=example.com \
  --set aws.certificateArn=arn:aws:acm:region:account:certificate/id \
  --set optionalServices.typesense.enabled=true \
  --set optionalServices.wppconnect.enabled=true
```

### Using S3 for File Storage

```bash
helm install my-jivas trueselph/jvcloud \
  --namespace jivas-demo \
  --create-namespace \
  --set domain.name=example.com \
  --set aws.certificateArn=arn:aws:acm:region:account:certificate/id \
  --set jivas.config.fileInterface=s3 \
  --set jivas.config.s3.bucketName=my-jivas-bucket \
  --set jivas.config.s3.regionName=us-east-1 \
  --set jivas.config.s3.accessKeyId=AKIAXXXXXXXXXXXXXXXX \
  --set jivas.config.s3.secretAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

### Using a Values File

Create a `custom-values.yaml` file:

```yaml
domain:
  name: example.com

aws:
  certificateArn: arn:aws:acm:region:account:certificate/id

optionalServices:
  typesense:
    enabled: true
```

Then install with:

```bash
helm install my-jivas trueselph/jvcloud \
  --namespace jivas-demo \
  --create-namespace \
  -f custom-values.yaml
```

## 🔰 Contributing

- **🐛 [Report Issues](https://github.com/TrueSelph/jvcloud/issues)**: Submit bugs found or log feature requests for the `jvcloud` project.
- **💡 [Submit Pull Requests](https://github.com/TrueSelph/jvcloud/blob/main/CONTRIBUTING.md)**: Review open PRs, and submit your own PRs.

<details closed>
<summary>Contributing Guidelines</summary>

1. **Fork the Repository**: Start by forking the project repository to your GitHub account.
2. **Clone Locally**: Clone the forked repository to your local machine using a git client.
   ```sh
   git clone https://github.com/TrueSelph/jvcloud
   ```
3. **Create a New Branch**: Always work on a new branch, giving it a descriptive name.
   ```sh
   git checkout -b new-feature-x
   ```
4. **Make Your Changes**: Develop and test your changes locally.
5. **Commit Your Changes**: Commit with a clear message describing your updates.
   ```sh
   git commit -m 'Implemented new feature x.'
   ```
6. **Push to GitHub**: Push the changes to your forked repository.
   ```sh
   git push origin new-feature-x
   ```
7. **Submit a Pull Request**: Create a PR against the original project repository. Clearly describe the changes and their motivations.
8. **Review**: Once your PR is reviewed and approved, it will be merged into the main branch. Congratulations on your contribution!
</details>

<details open>
<summary>Contributor Graph</summary>
<br>
<p align="left">
    <a href="https://github.com/TrueSelph/jvcloud/graphs/contributors">
        <img src="https://contrib.rocks/image?repo=TrueSelph/jvcloud" />
   </a>
</p>
</details>

## 📜 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.