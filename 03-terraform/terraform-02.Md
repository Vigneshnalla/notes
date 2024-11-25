# Terraform Best Practices and Concepts

## `terraform init`
- This command initializes the Terraform environment.
- Based on the configurations in the `provider.tf` file, Terraform sets up local infrastructure settings to communicate with the specified provider.

---

## Best Practices for Storing Terraform State Files

### 1. **Avoid Storing State Files in Git**
- State files contain sensitive information (e.g., infrastructure details, credentials).
- Storing them in Git exposes this data to unauthorized access and unintended edits.

### 2. **Restrict Write Permissions**
- Grant only **read** permissions to the state file.
- Terraform should be the only entity allowed to modify it.

### 3. **Centralized Storage with Locking Mechanism**
- Use centralized storage solutions like:
  - **Amazon S3**
  - **Azure Blob Storage**
  - **Google Cloud Storage**
- Enable a **locking mechanism** (e.g., DynamoDB for S3 backend) to prevent simultaneous updates and ensure consistency.

---

## Modules
- whenever you have reusable code nothing that  the code is repeating multiple times its better to create our own moudles or use existing modules so that it help to reuse code and avoid duplication in your Terraform configurations.

---

## Disadvantages of Terraform

### 1. **State File as a Single Source of Truth**
- Terraform relies on the state file to track infrastructure changes.
- If the state file is:
  - **Lost:** The infrastructure management becomes unreliable.
  - **Corrupted/Outdated:** Terraform may fail to manage resources properly.

### 2. **Lack of Awareness of Manual Changes**
- Changes made directly via the cloud provider's console or API are not automatically detected by Terraform.
- Manual reconciliation is required to ensure state and actual infrastructure match.

---
