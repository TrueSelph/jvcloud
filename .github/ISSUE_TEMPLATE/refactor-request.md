---
name: Refactor Request
about: Request a refactor to any of the core components of the jvcloud Helm chart.
title: "[Refactor Request]: "
labels: 'refactor'
assignees: ''

---

## Summary
Provide a brief description of the Helm chart refactor you are proposing. Focus on the high-level goal of the change.

## Current Problem
Describe the current issues with the existing Helm chart. Examples:
- Poor maintainability of templates
- Inconsistent value naming or structure
- Compatibility issues with certain Kubernetes versions
- Resource management inefficiencies
- Deployment complexity

## Proposed Solution
Explain the refactor you are suggesting:
- Outline how the Helm templates will be reorganized or improved
- Specify any Helm best practices you aim to implement
- Highlight changes to values structure or defaults

## Expected Benefits
Describe the outcomes and benefits of this refactor:
- Enhanced chart maintainability
- Improved deployment reliability
- Better resource management
- Simplified configuration
- Increased compatibility

## Scope of Changes
List the components or templates that are likely to be impacted:
- [ ] Values schema
- [ ] Deployment templates
- [ ] Service templates
- [ ] Ingress configuration
- [ ] Secret management
- [ ] Optional components
- [ ] Helper functions

## Backward Compatibility
Will this refactor break existing deployments or require users to update their values files? If so, describe the migration path.

## Additional Notes
Include any additional context or examples that will help clarify the refactor. Add links to related issues, discussions, Helm documentation, or Kubernetes best practices, if applicable.
