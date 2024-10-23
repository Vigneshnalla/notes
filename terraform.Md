# Terraform Basics

- **Terraform** is an Infrastructure as Code (IaaC) tool using a **declarative** approach—what you declare is what you get.
- It’s **idempotent**, meaning running the same code multiple times won’t recreate resources if they already exist.
- Unlike shell scripting, which is **imperative** (step-by-step), Terraform focuses on the end state.

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
