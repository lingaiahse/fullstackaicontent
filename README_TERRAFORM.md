# Terraform Guide: Basics to Advanced

## Overview
This document covers Terraform from fundamentals to advanced patterns, using a complex real-world scenario and deployments across AWS and Azure.
It explains multi-environment management, stage promotion, shared module strategy, cookiecutter-style values, state backend design, rollback and revert practices, testing, security, drift detection, and operational best practices.

---

## Topics List

### Basic Topics
- Introduction to Infrastructure as Code (IaC)
- Terraform installation and CLI basics
- Providers, resources, and configuration syntax
- Variables, outputs, and locals
- Terraform lifecycle commands: `init`, `plan`, `apply`, `destroy`
- State file fundamentals
- Remote state backends
- Terraform workspaces
- Resource targeting and dependency graph
- Interpolation and expression language

### Intermediate Topics
- Terraform modules and reuse
- Module registry vs private modules
- Input variable validation and defaults
- Conditional resources with `count` and `for_each`
- Dynamic blocks and nested structures
- Data sources and lookups
- Environment-specific configuration patterns
- Shared state locking and concurrency
- Secret management and sensitive values
- Terraform CLI automation in pipelines

### Advanced Topics
- Multi-cloud deployments (AWS + Azure) with the same code base
- Remote backends: S3 + DynamoDB, Azure Storage + CosmosDB/Blob leases
- Terraform Cloud / Enterprise and workspaces
- Terragrunt and wrapper patterns
- Policy-as-Code with Sentinel / Open Policy Agent (OPA)
- Drift detection and remediation
- Immutable infrastructure and replacement strategies
- Rollback, revert, and disaster recovery workflows
- Multi-account and multi-subscription architecture
- Terraform state migration and versioning
- Custom providers and provider aliasing
- Advanced graphing and planning strategies

---

## Real-World Scenario: Multi-Tenant SaaS Platform

### Scenario Summary
A SaaS application consists of:
- React frontend hosted on AWS S3 + CloudFront and on Azure Static Web Apps
- Python FastAPI backend deployed to AWS EKS and Azure Kubernetes Service (AKS)
- PostgreSQL databases in Amazon RDS and Azure Database for PostgreSQL
- Redis cache and Azure Cache for Redis
- API Gateway on AWS and Azure API Management
- Monitoring with Datadog / Azure Monitor and centralized logging
- Multi-environment architecture: `dev`, `staging`, `prod`
- CI/CD pipelines with GitHub Actions and Azure DevOps
- Support for feature branches, preview environments, and blue/green deploys

### Why Terraform?
Terraform allows us to:
- define infrastructure as code
- manage multi-cloud resources with a consistent model
- reuse modules across environments
- enforce environment separation
- version changes and collaborate safely with remote state
- rollback using plan history and automated promotions

---

## 1. Terraform Basics

### 1.1 What is Terraform?
Terraform is an open-source IaC tool by HashiCorp. It describes infrastructure resources in declarative configuration files and builds execution plans.

### 1.2 Installation and Setup
- Install from https://www.terraform.io/downloads
- Verify with `terraform version`
- Configure a working directory:
  - `main.tf` for resources
  - `variables.tf` for inputs
  - `outputs.tf` for outputs

### 1.3 Core file types
- `main.tf` — primary resource declarations
- `variables.tf` — input variables and descriptions
- `outputs.tf` — values exported after apply
- `providers.tf` — provider configuration
- `terraform.tfvars` / `.auto.tfvars` — environment values

### 1.4 Providers and Resources
A provider is a plugin for a cloud or service. Examples:
- AWS provider: `hashicorp/aws`
- Azure provider: `hashicorp/azurerm`

Example resource:
```hcl
provider "aws" {
  region = var.aws_region
}

resource "aws_s3_bucket" "frontend_assets" {
  bucket = "saas-frontend-${var.env}"
  acl    = "private"
}
```

### 1.5 Variables
Declare inputs:
```hcl
variable "env" {
  description = "Deployment environment"
  type        = string
  default     = "dev"
}
```

Use values:
```hcl
bucket = "app-${var.env}-${var.project_name}"
```

### 1.6 Outputs
```hcl
output "api_gateway_url" {
  description = "Endpoint for the backend API"
  value       = aws_api_gateway_deployment.api.invoke_url
}
```

### 1.7 Locals
Locals calculate values once and reuse them:
```hcl
locals {
  default_tags = {
    project = var.project_name
    env     = var.env
  }
}
```

### 1.8 Terraform Workflow
- `terraform init` — initialize directory and download providers
- `terraform plan` — preview changes
- `terraform apply` — execute changes
- `terraform destroy` — remove infrastructure

### 1.9 State
Terraform state tracks the real-world infrastructure.
- Local: `terraform.tfstate`
- Remote: cloud storage or Terraform Cloud

State ensures:
- resource mapping
- dependency management
- drift detection

### Interview Q&A: Basics
Q: What is Terraform state and why is it needed?
A: Terraform state stores metadata about deployed resources, tracks IDs, and preserves the dependency graph needed for diffs and future applies.

Q: Why use variables instead of hard-coded values?
A: Variables enable reusable configuration, environment-specific values, and safer management of secrets and credentials.

Q: What is the purpose of `terraform init`?
A: It initializes a working directory, downloads provider plugins, and sets up backends.

---

## 2. Environment Management and Multi-Stage Deployments

### 2.1 Environment Separation
Common pattern:
- `dev` for early testing
- `staging` for integration and QA
- `prod` for live traffic

Use separate directories or workspaces for each environment.

### 2.2 Workspaces vs Directory Layout
Workspaces store separate state within the same config.
Example:
- `terraform workspace new dev`
- `terraform workspace select prod`

Directory layout pattern:
```
infra/
  modules/
  envs/
    dev/
    staging/
    prod/
```

### 2.3 Cookiecutter Values and Parameterization
Use a repeatable pattern for shared values:
```hcl
variable "environment" {
  description = "Deployment stage"
  type        = string
}

variable "common_tags" {
  type = map(string)
  default = {
    owner   = "platform-team"
    project = "saas-app"
  }
}
```

Example `terragrunt.hcl` or `terraform.tfvars` per stage:
```hcl
environment = "staging"
aws_region  = "us-east-1"
app_version = "2026.05"
```

### 2.4 Deploying Multiple Environments
Best practice:
- Separate state backends per environment
- Separate variable files per environment
- Locked state during deploys
- Promote changes from dev → staging → prod

Example backend config for AWS:
```hcl
terraform {
  backend "s3" {
    bucket         = "saas-terraform-state"
    key            = "${var.env}/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "saas-terraform-lock"
    encrypt        = true
  }
}
```

Example backend config for Azure:
```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-state"
    storage_account_name = "tfstateaccount"
    container_name       = "tfstate"
    key                  = "${var.env}/terraform.tfstate"
  }
}
```

### 2.5 Promotion Model
- Run `terraform plan` in `dev`
- Review plan and apply
- Validate with automated tests
- Promote same version or config to `staging`
- After approval, deploy to `prod`

### Interview Q&A: Environments
Q: Why should `dev`, `staging`, and `prod` have separate state files?
A: Separate states isolate environments and prevent accidental cross-environment drift or resource reuse.

Q: What are Terraform workspaces good for?
A: Workspaces create separate state instances from the same configuration, helpful for simple environment separation or feature branch experiments.

Q: How do you parameterize environment-specific values?
A: Use variable definitions and separate `.tfvars` files, or wrapper tools like Terragrunt for environment-specific config.

---

## 3. Terraform Modules and Reuse

### 3.1 Module Basics
A module is a reusable package of Terraform configurations.

Example module structure:
```
modules/network/
  main.tf
  variables.tf
  outputs.tf
```

Use a module:
```hcl
module "vpc" {
  source      = "./modules/network"
  env         = var.env
  cidr_block  = var.vpc_cidr
  tags        = local.default_tags
}
```

### 3.2 Module Types
- Root module: current working directory
- Child module: referenced with `module` blocks
- Registry module: public or private module registry

### 3.3 Module Inputs and Outputs
Keep modules generic by exposing inputs and outputs.
Example inputs:
```hcl
variable "name" { type = string }
variable "subnet_cidrs" { type = list(string) }
```

Example outputs:
```hcl
output "subnet_ids" {
  value = aws_subnet.private[*].id
}
```

### 3.4 Complex Module Patterns
- Module composition: smaller modules used inside larger ones
- Environment-specific modules with shared core logic
- Feature modules for caching, database, or IAM

Example module call across clouds:
```hcl
module "database" {
  source    = "git::ssh://git@repo/modules/database.git?ref=v1.2.0"
  provider  = aws.db
  engine    = "postgres"
  version   = "14"
  env       = var.env
}
```

### 3.5 Module Versioning
Use semantic versioning and pin module sources.
Example:
```hcl
source = "git::https://github.com/org/terraform-modules.git//storage?ref=v1.0.0"
```

### Interview Q&A: Modules
Q: What are the benefits of using Terraform modules?
A: Modules improve reuse, enforce standards, and simplify complex infrastructure by splitting it into logical units.

Q: How do you make a module reusable across AWS and Azure?
A: Keep cloud-specific resources in separate modules, expose common inputs, and use provider aliases or wrapper modules for each cloud.

Q: When should you use a module registry?
A: When you want versioned, discoverable, and shareable infrastructure packages across teams.

---

## 4. Remote State and State Management

### 4.1 Why Remote State?
Remote state enables collaboration and locking. It stores the Terraform state in shared storage with version history.

### 4.2 AWS Remote State Backend
Example:
```hcl
terraform {
  backend "s3" {
    bucket         = "saas-terraform-state"
    key            = "${var.env}/terraform.tfstate"
    region         = var.aws_region
    dynamodb_table = "saas-terraform-lock"
    encrypt        = true
  }
}
```

Use `terraform state pull` to inspect current state and `terraform state list` to review resources.

### 4.3 Azure Remote State Backend
Example:
```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-state"
    storage_account_name = "tfstateaccount"
    container_name       = "tfstate"
    key                  = "${var.env}/terraform.tfstate"
  }
}
```

### 4.4 Backend Migration
If you move state from local to remote:
- `terraform init -migrate-state`
- Confirm state migration and versioning

### 4.5 State Locking
Use DynamoDB for AWS and Azure Blob leases for Azure to lock state during writes.
Example AWS DynamoDB table:
```hcl
resource "aws_dynamodb_table" "terraform_lock" {
  name         = "saas-terraform-lock"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"
  attribute {
    name = "LockID"
    type = "S"
  }
}
```

### 4.6 Sensitive Data and State
Terraform state may contain sensitive values unless marked `sensitive = true`. Use secrets managers or remote state encryption.

### Interview Q&A: State Management
Q: How does Terraform prevent concurrent state writes?
A: By using backend-specific locking, such as DynamoDB for S3 and Blob leases for Azure.

Q: What happens if state becomes corrupted?
A: You may restore from a backup, use `terraform state replace-provider`, or perform manual state repair carefully.

Q: What is `terraform state rm` used for?
A: It removes a resource from state without destroying it in the cloud, useful when importing or repairing drift.

---

## 5. Multi-Cloud Architecture: AWS + Azure

### 5.1 Provider Configuration
Example multiple providers:
```hcl
provider "aws" {
  alias  = "primary"
  region = var.aws_region
}

provider "azurerm" {
  features = {}
}
```

### 5.2 Multi-Cloud Resource Strategy
- Use provider-specific modules for AWS and Azure
- Share common naming, tagging, and security policies
- Keep cloud-specific resources isolated in module boundaries

Example cloud-specific module usage:
```hcl
module "aws_app" {
  source = "./modules/aws/app"
  providers = {
    aws = aws.primary
  }
  env = var.env
}

module "azure_app" {
  source = "./modules/azure/app"
  providers = {
    azurerm = azurerm
  }
  env = var.env
}
```

### 5.3 Networking and Security in AWS
- VPC with public/private subnets
- Security groups for EKS and RDS
- NAT gateways and Internet gateways
- IAM roles and policies

### 5.4 Networking and Security in Azure
- Virtual Network and subnets
- Network security groups (NSGs)
- Service principal authentication
- Role-based access control (RBAC)

### 5.5 Shared Data Sources
Use data sources for cross-module and cross-cloud lookup:
```hcl
data "aws_ami" "eks_worker" {
  most_recent = true
  owners      = ["602401143452"]
  filter {
    name   = "name"
    values = ["amazon-eks-node-*-v*"]
  }
}
```

### Interview Q&A: Multi-Cloud
Q: What is provider aliasing and why is it useful?
A: Aliasing allows multiple configurations of the same provider, useful when deploying resources in multiple regions or accounts.

Q: How can you share common code across AWS and Azure modules?
A: Use generic wrapper modules and shared variable formats while keeping cloud-specific implementations separate.

Q: What is the biggest challenge in multi-cloud Terraform deployments?
A: Managing differences in resource models, networking, security, and state across providers while retaining a consistent deployment workflow.

---

## 6. Advanced Configuration and Patterns

### 6.1 `count` and `for_each`
Create resources conditionally:
```hcl
resource "aws_lb_listener" "https" {
  count = var.enable_https ? 1 : 0
  # ...
}
```

Use `for_each` for maps or sets:
```hcl
resource "aws_security_group_rule" "ingress" {
  for_each = var.allowed_ports
  from_port = each.value
}
```

### 6.2 Dynamic Blocks
```hcl
resource "aws_security_group" "app" {
  dynamic "ingress" {
    for_each = var.ingress_rules
    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }
}
```

### 6.3 Conditionals and Ternaries
```hcl
instance_type = var.env == "prod" ? "m5.large" : "t3.medium"
```

### 6.4 Data Sources and External Data
Read existing cloud data:
```hcl
data "aws_vpc" "default" {
  default = true
}
```

External data provider example:
```hcl
data "external" "build_info" {
  program = ["python3", "scripts/get_build_info.py"]
}
```

### 6.5 Custom Providers and Aliasing
Use multiple provider configs for different accounts or regions.

Example:
```hcl
provider "aws" {
  alias  = "us_east"
  region = "us-east-1"
}

provider "aws" {
  alias  = "eu_west"
  region = "eu-west-1"
}
```

### 6.6 Policy-as-Code
Use Terraform Cloud or external tools to enforce:
- naming conventions
- tag requirements
- resource type restrictions
- encryption settings

Example OPA / Sentinel policies:
- require tags on all resources
- disallow public S3 buckets
- require HTTPS listeners

### 6.7 Drift Detection
Detect drift by comparing state to actual infrastructure.
- `terraform plan` will reveal drift
- Use scheduled checks in pipelines
- If drift is found, decide whether to accept or reconcile

### Interview Q&A: Advanced Patterns
Q: How do you use dynamic blocks effectively?
A: Use dynamic blocks to generate nested configuration based on complex input lists or maps, avoiding repetitive manual structure.

Q: When should you use `for_each` instead of `count`?
A: Use `for_each` for keyed collections and when each instance requires stable identity across changes.

Q: What is policy-as-code and why does it matter?
A: It enforces guardrails early in infrastructure deployment, preventing misconfigurations and security issues before resources are created.

---

## 7. State Management, Rollback, and Revert

### 7.1 State Versioning
Remote backends often keep historical state versions. Use this to restore after a failed change.

AWS S3 versioning example:
- enable versioning on the S3 bucket
- recover older `terraform.tfstate`

Azure Storage example:
- enable blob versioning or soft delete
- restore previous state blob

### 7.2 Rollback Strategies
Terraform does not have a native one-command rollback, but common patterns include:
- use version-controlled config and run a `terraform apply` on a prior commit
- restore remote state to a previous version and re-run apply
- use environment promotion and canary deployments to reduce blast radius

### 7.3 Safe Reverts
Steps for revert:
1. Identify the last known-good config in Git.
2. Check the remote state version and backup current state.
3. Checkout the prior commit or branch.
4. Run `terraform init` and `terraform plan`.
5. Confirm the plan matches expected revert changes.
6. Run `terraform apply`.

### 7.4 Recovering from Failed Apply
- If an apply fails partway, Terraform may leave partially created resources.
- Inspect the state with `terraform state list`
- Use `terraform state rm` for orphaned or broken resources
- Re-run `terraform apply` after cleanup

### 7.5 Audit and State History
- Use Terraform Cloud run logs or backend versioning
- Store state in secure, auditable storage
- Keep a consistent naming convention for backend keys

### Interview Q&A: Rollback and Revert
Q: How do you rollback Terraform infrastructure?
A: Revert the code to a prior version, use remote state history if needed, then apply the old configuration.

Q: Why is state versioning important?
A: It allows recovery from mistakes, audits changes, and supports reverting to a known-good infrastructure snapshot.

Q: What is the difference between rollback and destroy?
A: Rollback returns infrastructure to a previous configuration state; destroy removes resources entirely.

---

## 8. AWS Deployment Example

### 8.1 AWS Backend Architecture
For the SaaS app, AWS Terraform components might include:
- VPC, subnets, route tables, security groups
- EKS cluster, node groups, IAM roles
- RDS PostgreSQL instance and subnet groups
- Elasticache Redis
- S3 buckets for frontend, logs, and Terraform state
- CloudFront distribution
- API Gateway / Lambda or ALB
- IAM roles, policies, and service accounts

### 8.2 AWS Remote Backend Example
```hcl
terraform {
  backend "s3" {
    bucket         = "saas-terraform-state"
    key            = "${var.env}/terraform.tfstate"
    region         = var.aws_region
    dynamodb_table = "saas-terraform-lock"
    encrypt        = true
  }
}
```

### 8.3 AWS Example Module
Example `module "network"`:
```hcl
module "network" {
  source  = "./modules/aws/network"
  env     = var.env
  cidr    = var.vpc_cidr
  azs     = var.aws_azs
  tags    = local.default_tags
}
```

### 8.4 AWS Secrets and Secrets Manager
Use AWS Secrets Manager for DB passwords and third-party credentials:
```hcl
resource "aws_secretsmanager_secret" "db_password" {
  name = "${var.project_name}-${var.env}-db-password"
}
```

### Interview Q&A: AWS
Q: How do you lock Terraform state in AWS?
A: Use a DynamoDB table with the S3 backend and set `dynamodb_table` in the backend config.

Q: When would you choose ECS vs EKS in Terraform?
A: Choose EKS for Kubernetes workloads and complex orchestration; choose ECS for simpler container deployments with lower operational overhead.

---

## 9. Azure Deployment Example

### 9.1 Azure Backend Architecture
For the same SaaS app in Azure, Terraform resources may include:
- Resource groups and virtual networks
- AKS cluster with managed identities
- Azure Database for PostgreSQL
- Azure Cache for Redis
- Azure Storage account for state and frontend assets
- Azure API Management
- Application Gateway and WAF
- Log Analytics workspace and monitoring alerts

### 9.2 Azure Remote Backend Example
```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-state"
    storage_account_name = "tfstateaccount"
    container_name       = "tfstate"
    key                  = "${var.env}/terraform.tfstate"
  }
}
```

### 9.3 Azure Example Module
Example `module "aks"`:
```hcl
module "aks" {
  source              = "./modules/azure/aks"
  resource_group_name = azurerm_resource_group.main.name
  cluster_name        = "${var.project_name}-${var.env}-aks"
  dns_prefix          = "${var.project_name}-${var.env}"
  node_count          = var.aks_node_count
  tags                = local.default_tags
}
```

### 9.4 Azure Identity and RBAC
Use service principals or managed identities.
- `azurerm_client_config`
- `azurerm_role_assignment`

Example:
```hcl
data "azurerm_client_config" "current" {}

resource "azurerm_role_assignment" "aks_reader" {
  scope                = azurerm_resource_group.main.id
  role_definition_name = "Reader"
  principal_id         = data.azurerm_client_config.current.object_id
}
```

### Interview Q&A: Azure
Q: How do you enable blob versioning for Terraform state in Azure?
A: Enable versioning on the storage account and use the `azurerm` backend with a unique key per environment.

Q: Why use managed identities with AKS?
A: Managed identities reduce secret management by assigning Azure AD identities to cluster components.

---

## 10. CI/CD and GitOps

### 10.1 Git-Based Workflow
Use Git branches and pull requests for all infrastructure changes.
- feature branch → dev preview
- merge to main → staging apply
- approve → prod apply

### 10.2 GitHub Actions Example
Example workflow for Terraform:
```yaml
name: Terraform CI/CD
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: 1.9.0
      - name: Terraform Init
        run: terraform init -backend-config="env=${{ github.event.inputs.env }}"
      - name: Terraform Validate
        run: terraform validate
      - name: Terraform Plan
        run: terraform plan -var-file="envs/${{ github.event.inputs.env }}/terraform.tfvars"
      - name: Terraform Apply
        if: github.ref == 'refs/heads/main' && github.event_name == 'push'
        run: terraform apply -auto-approve -var-file="envs/${{ github.event.inputs.env }}/terraform.tfvars"
```

### 10.3 CI/CD Best Practices
- keep state backend config versioned separately from sensitive values
- run `terraform fmt` and `terraform validate`
- use plan review for pull requests
- separate plan and apply jobs
- only apply to prod on gated merge and approvals

### 10.4 GitOps and Pull Request Environments
- create preview stacks for branch-specific changes
- use directories or automation tools like Atlantis / Terraform Cloud

### Interview Q&A: CI/CD
Q: Why separate `plan` and `apply` in pipelines?
A: So review and approval can occur before executing changes, improving safety and auditability.

Q: What is Terraform Cloud workspace in CI/CD?
A: A remote environment that stores state, runs plans/applies, and can enforce policies as code.

---

## 11. Testing, QA, and Validation

### 11.1 Terraform Testing Tools
- `terraform validate` for syntax and provider checks
- `terraform fmt` for formatting
- `terraform plan` for visible changes
- `tflint` for linting best practices
- `checkov`, `tfsec` for security scanning

### 11.2 Automated Validation
Pipeline steps:
- `terraform fmt -check`
- `terraform validate`
- `tflint`
- `terraform plan`
- security scans

### 11.3 Test Environments
- Use isolated `dev` and `staging` accounts/subscriptions
- Deploy modules in small preview stacks
- Validate application behavior after infra changes

### Interview Q&A: Testing
Q: What does `terraform validate` check?
A: It checks syntax and internal consistency of the configuration, but not the actual cloud resources.

Q: How do security scanners help Terraform?
A: They catch misconfigurations, public exposures, and insecure defaults before deployment.

---

## 12. Security and Governance

### 12.1 Secrets Management
Never store secrets in plain `.tfvars` or source control.
- Use AWS Secrets Manager or Azure Key Vault
- Reference secrets through data sources or environment variables

### 12.2 Access Controls
- Use least privilege IAM roles in AWS
- Use Azure RBAC and managed identities
- grant only the necessary permissions for terraform service principals

### 12.3 Policy Enforcement
- Enforce tagging
- Disallow public access to storage
- Require encryption-at-rest and in-transit
- Use OPA / Sentinel policies or policy checks in CI

### Interview Q&A: Security
Q: How do you avoid storing secrets in Terraform state?
A: Use data sources to fetch secrets from a vault and mark outputs as sensitive; keep state backend encrypted.

Q: What is least privilege in Terraform context?
A: Grant Terraform only the permissions required to create and manage targeted resources, reducing blast radius.

---

## 13. Observability and Monitoring

### 13.1 Monitoring Infrastructure Changes
- Log Terraform runs in CI
- Use remote backend audit logs
- Store plan output as artifacts

### 13.2 Application Monitoring
- AWS CloudWatch metrics and alarms
- Azure Monitor alerts and Log Analytics
- Prometheus/Grafana for Kubernetes workloads

### 13.3 Integration with Ops Tools
- Push infrastructure metrics to Splunk or Datadog
- Track drift and configuration changes
- Alert on failed applies and workspace lock issues

### Interview Q&A: Observability
Q: How can Terraform changes be made observable?
A: Capture run logs, store plan artifacts, and integrate with monitoring and alerting systems.

Q: Why is drift detection important?
A: Drift detection ensures the deployed resources remain consistent with declared infrastructure, reducing config drift risk.

---

## 14. Operational Practices

### 14.1 Naming and Tagging Standards
Use consistent tags and naming conventions across AWS/Azure.
Example:
```hcl
tags = merge(local.default_tags, {
  environment = var.env
  team        = "platform"
})
```

### 14.2 Release Management
- Track Terraform changes in Git
- Attach plan outputs to release notes
- Use approval gates for production changes

### 14.3 Change Control
- Enforce PR reviews for Terraform code
- Approve plans before apply
- Use git history to audit changes

### 14.4 Runbook and Recovery
- Document how to restore state from backups
- Document how to rollback a bad deploy
- Keep a list of critical resources and state locations

---

## 15. Practical Example: Complex Multi-Environment Layout

### 15.1 Recommended Directory Structure
```
infra/
  modules/
    aws/
      network/
      eks/
      database/
    azure/
      network/
      aks/
      database/
  envs/
    dev/
      main.tf
      providers.tf
      variables.tf
      terraform.tfvars
    staging/
      main.tf
      providers.tf
      variables.tf
      terraform.tfvars
    prod/
      main.tf
      providers.tf
      variables.tf
      terraform.tfvars
  scripts/
    bootstrap.sh
```

### 15.2 Example `providers.tf`
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = ">= 5.0"
    }
    azurerm = {
      source  = "hashicorp/azurerm"
      version = ">= 3.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
  alias  = "default"
}

provider "azurerm" {
  features = {}
}
```

### 15.3 Example `main.tf`
```hcl
module "network_aws" {
  source = "../../modules/aws/network"
  env    = var.env
  cidr   = var.aws_vpc_cidr
}

module "network_azure" {
  source = "../../modules/azure/network"
  env    = var.env
  cidr   = var.azure_vnet_cidr
}
```

### 15.4 Example `terraform.tfvars`
```hcl
env          = "staging"
aws_region   = "us-east-1"
azure_region = "eastus"
project_name = "saas-platform"
```

### 15.5 Example `backend.tf`
For AWS:
```hcl
terraform {
  backend "s3" {
    bucket         = "saas-terraform-state"
    key            = "${var.env}/terraform.tfstate"
    region         = var.aws_region
    dynamodb_table = "saas-terraform-lock"
    encrypt        = true
  }
}
```

For Azure:
```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-state"
    storage_account_name = "tfstateaccount"
    container_name       = "tfstate"
    key                  = "${var.env}/terraform.tfstate"
  }
}
```

---

## 16. Common Terraform Commands
- `terraform init`
- `terraform fmt -recursive`
- `terraform validate`
- `terraform plan -var-file=terraform.tfvars`
- `terraform apply -var-file=terraform.tfvars`
- `terraform state list`
- `terraform state show <address>`
- `terraform import <address> <id>`
- `terraform workspace list`
- `terraform destroy`

---

## 17. Migration and Advanced Operations

### 17.1 Migrating State Between Backends
- Configure the new backend in `backend.tf`
- Run `terraform init -migrate-state`
- Verify the migrated state and remove old backend reference if needed

### 17.2 Importing Existing Resources
If resources already exist:
```bash
terraform import aws_s3_bucket.frontend my-bucket-name
```

### 17.3 Resource Renaming
Use `terraform state mv` to rename resources without destroying them.

### 17.4 Handling Provider Upgrades
- Pin provider versions in `required_providers`
- Test provider upgrades in a non-production environment first
- Review provider changelog and plan output carefully

---

## 18. Practical AWS + Azure Rollback Example

### 18.1 Rollback with Git and State History
1. Identify the last good commit in Git.
2. Checkout that commit.
3. Run `terraform init` and `terraform plan`.
4. Confirm a plan that removes or restores changed resources.
5. Apply the plan to revert infrastructure.

### 18.2 State Restoration Example
If AWS state is corrupted, restore an S3 object version or use a backup from DynamoDB.
For Azure, restore the previous blob version in the storage account.

### 18.3 Example Revert Scenario
A bad config change created the wrong subnet.
- `git checkout <good-commit>`
- `terraform apply -var-file=prod.tfvars`
- Validate via AWS / Azure console

---

## 19. Recommended Terraform Best Practices
- Keep root modules small and simple
- Use modules for repeatable patterns
- Isolate environments with separate states
- Store sensitive values in vaults
- Use remote state and locking
- Review plans before apply
- Automate formatting, validation, and security scans
- Keep provider and module versions pinned
- Document backend configuration and team workflows

---

## 20. Interview Questions: Terraform End-to-End
Q: What is the difference between `terraform apply` and `terraform plan`?
A: `terraform plan` previews changes; `terraform apply` executes them.

Q: How do you manage multiple environments in Terraform?
A: Use separate state backends, separate variable files, workspaces, or wrapper tools like Terragrunt.

Q: What are remote backends and why use them?
A: Remote backends store state centrally, enable collaboration, locking, and state versioning.

Q: How can Terraform support both AWS and Azure in the same project?
A: Configure both providers, use provider aliases, and separate cloud-specific modules while sharing common variable definitions.

Q: What is a Terraform module and how does it improve infrastructure code?
A: A module is reusable configuration. It improves maintainability, consistency, and reuse across environments.

Q: How do you handle secrets securely in Terraform?
A: Use vault services for secret storage, avoid hard-coded secrets in `.tfvars`, mark inputs/outputs as sensitive, and secure state storage.

Q: What is Terraform drift and how do you correct it?
A: Drift is when actual infrastructure differs from declared config. Correct it by reviewing `terraform plan` and reapplying the desired configuration or importing external changes.

Q: How do you rollback an infrastructure change with Terraform?
A: Revert code to a prior state in Git, then run `terraform plan` and `terraform apply`, optionally using remote state history.

---

## 21. Conclusion
This guide offers a Terraform foundation that scales from simple deployments to complex AWS and Azure multi-environment architectures.
Use modular design, remote state, environment separation, and CI/CD automation to safely deploy and operate infrastructure as code.

> For real-time SaaS systems, the combination of Terraform, cloud-specific modules, and strong operational processes is the key to repeatable, auditable, and resilient infrastructure.
