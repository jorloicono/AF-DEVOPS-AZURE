# 🌩️ Terraform on Azure: Create Blob Storage (Step-by-Step Guide)

## ✅ Prerequisites

- An active **Azure subscription**
- **Terraform** installed → [Download Terraform](https://www.terraform.io/downloads.html)
- **Azure CLI** installed → [Install Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
- Login to Azure:
  ```bash
  az login
````

## 📁 Step 1: Create a Project Directory

```bash
mkdir terraform-azure-blob
cd terraform-azure-blob
```


## 📄 Step 2: Create Terraform Configuration Files

### 1. `main.tf`

```hcl
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "rg-blob-example"
  location = "East US"
}

resource "azurerm_storage_account" "example" {
  name                     = "examplestoracct01"  # Must be globally unique
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "example" {
  name                  = "example-container"
  storage_account_name  = azurerm_storage_account.example.name
  container_access_type = "private"
}
```

### 2. `variables.tf` (optional)

```hcl
variable "location" {
  default = "East US"
}
```

### 3. `outputs.tf` (optional)

```hcl
output "storage_account_name" {
  value = azurerm_storage_account.example.name
}

output "container_name" {
  value = azurerm_storage_container.example.name
}
```

---

## 🛠️ Step 3: Initialize Terraform

```bash
terraform init
```

---

## 🔍 Step 4: Preview the Plan

```bash
terraform plan
```

---

## 🚀 Step 5: Apply the Configuration

```bash
terraform apply
```

Type `yes` when prompted.

---

## 🧹 Step 6: Destroy the Resources

```bash
terraform destroy
```

---


