# Terraform Basics

- **Terraform** is an Infrastructure as Code (IaaC) tool using a **declarative** approach—what you declare is what you get.
- It’s **idempotent**, meaning running the same code multiple times won’t recreate resources if they already exist.

## State in Terraform

- **State** is how Terraform keeps track of what’s been created.
- It compares the **declared state** (what’s in your Terraform files) to the **actual state** (resources created in the provider, e.g., AWS).

## How Terraform Tracks Changes

- After running Terraform, it creates a file called `terraform.tfstate`.
  - This file records the actual state of your infrastructure.
- If you run Terraform again, it compares the **declared state** to the **actual state** recorded in `terraform.tfstate`.
- If changes are detected (e.g., if you manually delete something in AWS), Terraform will **refresh the state** to reconcile differences and update the infrastructure as needed.

## Collaboration Challenges

- In a team, using local `terraform.tfstate` files can lead to issues:
  1. **Risk of manual edits/deletion** of the local state file.
  2. **No coordination** in a collaborative environment—multiple people might apply changes simultaneously.
  3. Potential for **duplicate resources or errors**.

## Remote State

- **Remote State** helps avoid these problems by storing the `terraform.tfstate` in a shared, centralized location.
- It includes **locking mechanisms**, so if one person is making changes, others are temporarily prevented from applying their changes, avoiding conflicts.

## Example of Remote State: S3

- Store `terraform.tfstate` in an **S3 bucket**.
- Use a **DynamoDB table** for locking, ensuring only one person can modify the infrastructure at a time.

## Local State vs. Remote State

1. **Local State** risks accidental edits or deletions and doesn’t support collaboration well.
2. **Remote State** is centralized, safe, and supports team workflows with locking to prevent conflicts.


# Creating Multiple Environments with Terraform

## Overview
To manage multiple environments (e.g., **DEV**, **QA**, **PROD**, **UAT**) in Terraform, you can use the same code base with environment-specific configurations. This is typically achieved using **`.tfvars`** files to override default values and manage environment-specific settings.

## Using `.tfvars` Files

- **Code is the same** for all environments—resource definitions remain consistent.
- Use `.tfvars` files to specify values that differ between environments (e.g., database names, URLs, instance types).
- This allows you to reuse the same Terraform configuration for environments like **DEV** and **PROD**, while changing only the variables.

### Example Environments
- **DEV** (Development)
- **QA** (Quality Assurance)
- **PROD** (Production)
- **UAT** (User Acceptance Testing)

## Managing Environments with `.tfvars`

### 1. **Default Variables**
You can define variables in a `variables.tf` file with default values. These values can be overridden by environment-specific `.tfvars` files.

**Example `variables.tf`:**
```hcl
variable "environment" {
  description = "The environment to deploy to (dev, qa, prod, uat)"
  default     = "dev"
}

variable "db_name" {
  description = "Database name"
  default     = "db-dev"
}

variable "backend_name" {
  description = "Backend service name"
  default     = "backend-dev"
}

variable "frontend_name" {
  description = "Frontend service name"
  default     = "frontend-dev"
}
```
# Managing Multiple Environments with Terraform

## Environment-Specific `.tfvars` Files

Create `.tfvars` files for each environment to override the default variables.

### Example `dev.tfvars`:
```hcl
environment   = "dev"
db_name       = "db-dev"
backend_name  = "backend-dev"
frontend_name = "frontend-dev"
domain        = "daws78s.online"
```
### Example `prod.tfvars`:
```hcl
environment   = "prod"
db_name       = "db-prod"
backend_name  = "backend-prod"
frontend_name = "frontend-prod"
domain        = "daws78s.online"
```
## S3 Backend Configuration for Remote State
To manage state separately for each environment, use different S3 buckets for each environment's state file.


Example Backend Configuration:
In main.tf:
```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state-${var.environment}"
    key            = "state/terraform.tfstate"
    region         = "us-west-2"
    dynamodb_table = "terraform-lock-table"
  }
}

## For DEV environment:
```hcl
terraform apply -var-file="dev.tfvars"
```
## For PROD environment:
```bash 
terraform apply -var-file="prod.tfvars"
```
## terraform init -upgrade

- The command `terraform init -upgrade` is used to reinitialize your Terraform working directory and update any provider plugins to their latest versions.

- For example:
  - I have code in a Git repository, and my Terraform code is pointing to this.
  - If I run `terraform init`, it downloads the code from Git and places it in `.terraform/modules` inside it.

- When I make changes in Git and try to see those in Terraform, the changes will not be reflected. However, if I run this cmd `terraform init -upgrade`, it will update the modules.
  
  ```bash
  $ terraform init -upgrade
  Initializing the backend...
  Upgrading modules...
  Downloading git::https://github.com/Vigneshnalla/terraform-aws-ec2.git?ref=main for ec2-test...       



terraform init -backend-config=dev/backend.tfvars
terraform plan -var-file=dev/dev.tfvars

terraform init -backend-config=dev/backend.tfvars
If fields are missing in the backend configuration, terraform init will raise an error, pointing to the missing fields required for the backend to function properly. Make sure all fields required by your chosen backend are provided in the backend.tf file before running the init command.

and for var files
terraform plan -var-file=dev/dev.tfvars

if you try to add backend.config again it gives error backend configuration changed

so we need to reconfigure

terraform init -reconfigure -backend-config=prod/backend.tf

this is for pointing to bucket belongs to paritcular environment

terraform destroy -var-file=prod/prod.tfvars -auto-approve