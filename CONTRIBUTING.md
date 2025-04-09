# Contributing to JVCloud

Thank you for your interest in contributing to JVCloud! This document provides guidelines and instructions for contributing.

## Code of Conduct

By participating in this project, you agree to abide by our Code of Conduct. Please read it before contributing.

## How Can I Contribute?

### Reporting Bugs

This section guides you through submitting a bug report. Following these guidelines helps maintainers understand your report, reproduce the behavior, and find related reports.

* **Use a clear and descriptive title** for the issue to identify the problem.
* **Describe the exact steps which reproduce the problem** in as many details as possible.
* **Provide specific examples to demonstrate the steps**. Include links to files or GitHub projects, or copy/pasteable snippets, which you use in those examples.
* **Describe the behavior you observed after following the steps** and point out what exactly is the problem with that behavior.
* **Explain which behavior you expected to see instead and why.**
* **Include screenshots and animated GIFs** if possible.
* **If the problem wasn't triggered by a specific action**, describe what you were doing before the problem happened.

### Suggesting Enhancements

This section guides you through submitting an enhancement suggestion, including completely new features and minor improvements to existing functionality.

* **Use a clear and descriptive title** for the issue to identify the suggestion.
* **Provide a step-by-step description of the suggested enhancement** in as many details as possible.
* **Provide specific examples to demonstrate the steps or point out the part of JVCloud which the suggestion is related to**.
* **Describe the current behavior** and **explain which behavior you expected to see instead** and why.
* **Explain why this enhancement would be useful** to most JVCloud users.

### Pull Requests

* **Fill in the required template**
* **Do not include issue numbers in the PR title**
* **Include screenshots and animated GIFs in your pull request whenever possible**
* **Follow the coding style used throughout the project**
* **Include appropriate test cases**
* **Document new code**
* **End all files with a newline**

## Development Workflow

1. Fork the repository
2. Create a new branch: `git checkout -b my-branch-name`
3. Make your changes
4. Push to your fork and submit a pull request
5. Wait for your pull request to be reviewed and merged

### Development Environment Setup

#### Prerequisites

* Kubernetes cluster (v1.19+)
* Helm 3
* Access to a Docker registry (for custom image builds)

#### Testing Changes

Before submitting your changes, make sure to test them with:

```bash
# Install chart with your changes
helm install test-jvcloud ./jvcloud/helm/jvcloud \
  --namespace test-jvcloud \
  --create-namespace \
  --set domain.name=example.com \
  --set aws.certificateArn=arn:aws:acm:region:account:certificate/id

# Verify deployment
kubectl get pods -n test-jvcloud

# Check logs
kubectl logs -n test-jvcloud deployment/jivas

# Clean up
helm uninstall test-jvcloud -n test-jvcloud
kubectl delete namespace test-jvcloud
```

## Style Guides

### Git Commit Messages

* Use the present tense ("Add feature" not "Added feature")
* Use the imperative mood ("Move cursor to..." not "Moves cursor to...")
* Limit the first line to 72 characters or less
* Reference issues and pull requests liberally after the first line
* Consider starting the commit message with an applicable emoji:
    * 🎨 `:art:` when improving the format/structure of the code
    * 🐎 `:racehorse:` when improving performance
    * 🚱 `:non-potable_water:` when plugging memory leaks
    * 📝 `:memo:` when writing docs
    * 🐛 `:bug:` when fixing a bug
    * 🔥 `:fire:` when removing code or files
    * 💚 `:green_heart:` when fixing the CI build
    * ✅ `:white_check_mark:` when adding tests
    * 🔒 `:lock:` when dealing with security
    * ⬆️ `:arrow_up:` when upgrading dependencies
    * ⬇️ `:arrow_down:` when downgrading dependencies

### Helm Chart Style Guide

* Use 2 spaces for indentation
* Use quotes for string values
* Add comments for non-obvious values
* Group related values together

## License

By contributing, you agree that your contributions will be licensed under the project's license.