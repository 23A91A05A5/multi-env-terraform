# Manage Multi-Environment Cloud Infrastructure with Terraform Workspaces

## Overview
This project demonstrates how to design and manage multi-environment cloud infrastructure (dev, staging, production) using Terraform workspaces from a single, modular codebase. It follows Infrastructure as Code (IaC) best practices and focuses on state isolation, environment-specific configurations, and CI/CD automation.

The infrastructure is provisioned on AWS using Terraform with a remote backend and automated workflows.

---

## Project Structure

```
multi-env-terraform/
├── backend.tf
├── main.tf
├── variables.tf
├── locals.tf
├── outputs.tf
├── dev.tfvars
├── staging.tfvars
├── production.tfvars
├── modules/
│   ├── networking/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── compute/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
└── .github/
    └── workflows/
        ├── terraform.yml
        └── terraform-prod-apply.yml
```

---

## Environments and Workspaces

Terraform workspaces are used to manage three environments:

- dev
- staging
- production

Dynamic naming is implemented using the active workspace:

```
app-dev
app-staging
app-production
```

Each environment has its own `.tfvars` file to control configuration differences such as instance size and CIDR ranges.

---

## Remote State Management

Terraform state is stored remotely using:

- S3 bucket for state storage
- DynamoDB table for state locking

This ensures:
- State isolation per environment
- Prevention of concurrent state changes
- Safe team collaboration

---

## Prerequisites

- Terraform (latest stable version)
- AWS CLI
- AWS account
- GitHub account

---

## Setup Instructions

### 1. Initialize Terraform
```
terraform init
```

### 2. Create Workspaces
```
terraform workspace new dev
terraform workspace new staging
terraform workspace new production
```

### 3. Deploy Infrastructure

#### Dev Environment
```
terraform workspace select dev
terraform plan -var-file=dev.tfvars
terraform apply -var-file=dev.tfvars
```

Repeat the same steps for `staging` and `production` using their respective variable files.

---

## CI/CD Pipeline

GitHub Actions is used for CI/CD automation.

### Pull Request Workflow
On every pull request to the `main` branch:
- Terraform is initialized
- Terraform configuration is validated
- Terraform plan is generated automatically

### Production Deployment
Production deployments are handled via a separate workflow that:
- Requires manual approval
- Uses GitHub Environments for protection
- Prevents accidental production changes

---

## Security and Best Practices

- No secrets or credentials are hardcoded
- AWS IAM is used to restrict state access
- Production changes require manual approval
- Remote state locking is enabled

---

## Outputs

Terraform outputs provide useful information such as:
- Public IP address of EC2 instances

Retrieve outputs using:
```
terraform output
```

---

## Architecture Diagram

```
User
 |
 v
EC2 Instance
 |
 v
VPC
```

(Replace with a visual diagram image for final submission.)

---

## Submission Checklist

- Terraform code pushed to GitHub
- CI/CD pipeline execution screenshots
- Production approval screenshot
- Architecture diagram
- README.md documentation

---

## Conclusion

This project demonstrates a complete multi-environment infrastructure solution using Terraform workspaces, remote state management, CI/CD automation, and secure deployment practices suitable for modern DevOps workflows.
