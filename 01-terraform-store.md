## Creating a Storage Account for Terraform State File

### Overview
This document outlines the steps to provision an Azure Storage Account to store Terraform state files. Using an Azure Storage Account allows for secure, centralized state management when working with Terraform in a cloud environment.

### Prerequisites
- You must have access to the Azure CLI (`az` command).
- Ensure you are logged in to your Azure account via the CLI with the necessary permissions to create resource groups and storage accounts.

### Steps to Create the Storage Account

#### 1. Create a Working Directory
First, create a directory where you will store the script for creating the Terraform storage account.

```bash
mkdir cloud-labs-scripts
```

Navigate into the newly created directory:
```bash
cd cloud-labs-scripts/
```

#### 2. Create the Shell Script
Next, create a shell script named `create-terraform-storage.sh` to automate the process of setting up the resource group, storage account, and blob container.

```bash
vi create-terraform-storage.sh
```

In the file, add the following script:
```bash
#!/bin/sh

RESOURCE_GROUP_NAME="cloud-labs-rg"
STORAGE_ACCOUNT_NAME="cloudlabssa00"

# Create Resource Group
az group create -l southindia -n $RESOURCE_GROUP_NAME

# Create Storage Account
az storage account create -n $STORAGE_ACCOUNT_NAME -g $RESOURCE_GROUP_NAME -l southindia --sku Standard_LRS

# Create Storage Account Blob Container
az storage container create --name tfstate --account-name $STORAGE_ACCOUNT_NAME
```

Save and exit the editor.

#### 3. Execute the Script
Now, run the script to provision the Azure resources.

```bash
sh create-terraform-storage.sh
```

This will:
1. **Create the Resource Group**: A resource group named `cloud-labs-rg` will be created in the `southindia` region.
2. **Create the Storage Account**: A storage account named `cloudlabssa00` will be created in the `southindia` region, using `Standard_LRS` for redundancy.
3. **Create the Blob Container**: A blob container named `tfstate` will be created in the storage account for storing the Terraform state files.

#### 4. Verify the Resources
Once the script completes successfully, you can verify the created resources by checking them via the Azure portal or using the Azure CLI.

To verify the storage account and blob container:
```bash
az storage account show -n cloudlabssa00 -g cloud-labs-rg
```

To list the containers:
```bash
az storage container list --account-name cloudlabssa00
```

### Conclusion
You have now created a resource group, a storage account, and a blob container for storing your Terraform state files. The storage account can now be used in your Terraform configuration to securely manage and store state files.

--- 

This document serves as a guide to automate the process of creating necessary Azure resources for Terraform state file storage.

