# Setting Up Azure DevOps Organization, Project, and Service Principal for Service Connection

## 1. **Azure DevOps Organization and Project Creation**

### Step 1.1: Create Azure DevOps Organization
- **Organization Name**: `cloud-labs-org`
- An Azure DevOps Organization allows you to manage your DevOps projects, pipelines, and repos.
  
  You can create an organization by navigating to [Azure DevOps](https://dev.azure.com/) and following these steps:
  1. Click on **Create new organization**.
  2. Follow the on-screen prompts to set up your new organization.

### Step 1.2: Create Azure DevOps Project
- **Project Name**: `cloud-labs-proj`
  
  Once the organization is created:
  1. In the Azure DevOps portal, click **New Project**.
  2. Enter the project name `cloud-labs-proj`.
  3. Set the project visibility to **Private** to restrict access.
  4. Click **Create** to finalize the project creation.

---

## 2. **Creating a Service Principal Using Azure CLI**

To authenticate Azure DevOps with Azure services, a Service Principal (SP) is needed. Below are the steps to create a Service Principal and assign it the **Contributor** role.

### Step 2.1: Login to Azure via Azure CLI

From your terminal, log in to Azure using the command:

```bash
az login
```
- This will prompt you to authenticate with your Azure credentials.

### Step 2.2: Set Your Subscription (If Necessary)

If you have multiple subscriptions, ensure the correct one is set:

```bash
az account set --subscription <subscription-id>
```

You can retrieve your subscription ID using:

```bash
az account list --output table
```

### Step 2.3: Create Service Principal

To create a Service Principal, run the following command:

```bash
az ad sp create-for-rbac --name "cloud-labs-sp" --role Contributor --scopes /subscriptions/XXXX-XXXX-XXXX-XXXX
```

#### Output:
```
Creating 'Contributor' role assignment under scope '/subscriptions/XXXX-XXXX-XXXX-XXXX'
The output includes credentials that you must protect. Be sure that you do not include these credentials in your code or check the credentials into your source control. For more information, see https://aka.ms/azadsp-cli
{
  "appId": "XXXX-XXXX-XXXX-XXXX",
  "displayName": "cloud-labs-sp",
  "password": "XXXX-XXXX-XXXX-XXXX",
  "tenant": "XXXX-XXXX-XXXX-XXXX"
}
```

- **appId**: This is the Client ID of the Service Principal.
- **password**: This is the Client Secret, which you must store securely.
- **tenant**: This is the Tenant ID of your Azure Active Directory (AAD).

### Step 2.4: Verify Role Assignment

To verify that the Service Principal was assigned the **Contributor** role, use the following command:

```bash
az role assignment list --assignee <appId> --all --output table
```

#### Sample Output:
```
Principal                             Role         Scope
------------------------------------  -----------  ---------------------------------------------------
XXXX-XXXX-XXXX-XXXX Contributor  /subscriptions/XXXX-XXXX-XXXX-XXXX
```

---

## 3. **Creating a Service Connection in Azure DevOps**

Now that the Service Principal is created, the next step is to use it to create a service connection in Azure DevOps.

### Step 3.1: Navigate to Service Connections
1. In your Azure DevOps organization (`cloud-labs-org`), go to **Project Settings** in your project (`cloud-labs-proj`).
2. Under **Pipelines**, click on **Service connections**.

### Step 3.2: Create a New Service Connection
1. Click on **New service connection**.
2. Choose **Azure Resource Manager**.
3. Select **Service principal (manual)**.

### Step 3.3: Enter Service Principal Details
- **Subscription ID**: `XXXX-XXXX-XXXX-XXXX`
- **Subscription name**: Your subscription name (retrieved during the subscription setting).
- **Service principal ID** (Client ID): `XXXX-XXXX-XXXX-XXXX`
- **Service principal key** (Client Secret): `XXXX-XXXX-XXXX-XXXX`
- **Tenant ID**: `XXXX-XXXX-XXXX-XXXX`

### Step 3.4: Finalize and Verify
1. Give the service connection a relevant name like `cloud-labs-sp-connection`.
2. Ensure that **Grant access permission to all pipelines** is checked if you want this connection to be available across all pipelines in the project.
3. Click **Verify and Save**.

---

## 4. **Summary of Steps**

- **Created Azure DevOps Organization**: `cloud-labs-org`.
- **Created Private Project**: `cloud-labs-proj`.
- **Created Service Principal**: `cloud-labs-sp` with **Contributor** role scoped to the subscription.
- **Configured a Service Connection** in Azure DevOps using the Service Principal.

---

This document provides a clear outline of how you set up your Azure DevOps organization, project, and service principal, which is now used to authenticate Azure services within Azure DevOps pipelines.
