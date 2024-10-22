---

### FAQ Document: Common Issues and Fixes for Terraform Storage Setup

#### 1. **Error: Invalid Storage Account Name**

**Problem**:  
When running the script, you might encounter the following error:
```bash
(AccountNameInvalid) cloudlabsstorageaccount00 is not a valid storage account name. Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
```

**Cause**:  
This error occurs when the storage account name doesn't adhere to Azure's naming conventions. The name must:
- Be between 3 and 24 characters.
- Consist only of lowercase letters and numbers (no uppercase letters, spaces, or special characters).

**Solution**:  
Update the storage account name to follow the required format. For example:
```bash
STORAGE_ACCOUNT_NAME="cloudlabssa00"
```

Make sure the name is within the character limits and consists of only lowercase letters and numbers.

---

#### 2. **Message: No Credentials Provided for Storage Account Access**

**Message**:  
During the execution of the script, you may see this informational message:
```bash
There are no credentials provided in your command and environment. We will query for account key for your storage account.
```

**Explanation**:  
This is not an error, but a notification from Azure CLI indicating that it's attempting to access the storage account without explicitly provided credentials. In this case, it successfully used your current authenticated session for authorization.

**Outcome**:  
Since the container was successfully created and visible in the Azure portal, no additional action is needed. The Azure CLI automatically managed the authentication using the session under which you were logged in.

**Optional Actions**:  
If you want to explicitly control authentication, you can use one of the following approaches:
- **Use `--auth-mode login`** to leverage your Azure AD login:
  ```bash
  az storage container create --name tfstate --account-name $STORAGE_ACCOUNT_NAME --auth-mode login
  ```
- **Query and provide the account key**:
  ```bash
  ACCOUNT_KEY=$(az storage account keys list --resource-group $RESOURCE_GROUP_NAME --account-name $STORAGE_ACCOUNT_NAME --query '[0].value' --output tsv)
  az storage container create --name tfstate --account-name $STORAGE_ACCOUNT_NAME --account-key $ACCOUNT_KEY
  ```

---

