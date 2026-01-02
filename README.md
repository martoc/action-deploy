[![checks](https://github.com/martoc/action-deploy/actions/workflows/checks.yml/badge.svg?branch=main&event=push)](https://github.com/martoc/action-deploy/actions/workflows/checks.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# action-deploy

A GitHub Action that deploys workloads to AWS using CloudFormation. It handles ECR repository creation, container image distribution, and CloudFormation stack deployment.

## Features

- Automatic ECR repository creation
- Container image distribution to target ECR
- CloudFormation stack deployment with parameter injection
- Environment-specific configuration support

## Quick Start

```yaml
- name: Deploy
  uses: martoc/action-deploy@v0
  env:
    TAG_VERSION: ${{ github.sha }}
  with:
    region: us-east-2
    environment: production
    container-registry-namespace: myorg
    workload-name: my-service
```

## Documentation

- [Usage Guide](./docs/USAGE.md) - Detailed usage instructions and examples
- [Code Style](./docs/CODESTYLE.md) - Code style guidelines for contributors

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `region` | AWS region for deployment | Yes |
| `environment` | Deployment environment | Yes |
| `container-registry-namespace` | Container registry namespace | No |
| `workload-name` | Name of the workload | Yes |
| `workload-type` | Type of workload | No |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `TAG_VERSION` | Version tag for the container image (required) |

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
