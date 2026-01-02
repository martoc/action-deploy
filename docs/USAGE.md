# Usage

## Overview

This GitHub Action deploys workloads to AWS using CloudFormation. It handles ECR repository creation, container image distribution, and CloudFormation stack deployment.

## Prerequisites

### Secrets Configuration

Before using this action, ensure you have the following configured:

- **AWS Credentials**: Configure AWS credentials with appropriate permissions using `aws-actions/configure-aws-credentials`
- **TAG_VERSION**: Environment variable containing the version to deploy

### Required Permissions

The AWS IAM role must have permissions for:

- ECR: `CreateRepository`, `DescribeRepositories`, `GetAuthorizationToken`, `BatchCheckLayerAvailability`, `PutImage`
- CloudFormation: `CreateStack`, `UpdateStack`, `DescribeStackEvents`
- IAM: `CAPABILITY_IAM` for stack resources

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `region` | AWS region for deployment | Yes | - |
| `environment` | Deployment environment (e.g., dev, staging, prod) | Yes | - |
| `container-registry-namespace` | Container registry namespace | No | - |
| `workload-name` | Name of the workload to deploy | Yes | - |
| `workload-type` | Type of workload being deployed | No | - |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `TAG_VERSION` | Version tag for the container image (required) |

## Example

```yaml
name: Deploy Workload

on:
  push:
    branches:
      - main

jobs:
  deploy:
    permissions:
      contents: write
      id-token: write
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_TO_ASSUME }}
          role-session-name: github-actions
          aws-region: us-east-2

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

## Deployment Process

1. **ECR Setup**: Creates ECR repository if it doesn't exist
2. **Image Distribution**: Pulls the container image and pushes to target ECR
3. **CloudFormation Deployment**: Deploys the stack using parameters from `etc/cloud/aws/{account_id}/{region}/{environment}/parameters.json`

## Directory Structure

The action expects the following structure:

```text
├── etc/
│   └── cloud/
│       └── aws/
│           └── {account_id}/
│               └── {region}/
│                   └── {environment}/
│                       └── parameters.json
└── cloudformation-template/
    └── src/
        └── cloudformation/
            └── main.yaml
```
