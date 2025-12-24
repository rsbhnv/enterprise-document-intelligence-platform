# Full Documentation Template – Azure Key Vault + Databricks + Secret Scopes

This document is a **professional template** showcasing hands-on knowledge of setting up **Azure Key Vault**, integrating it with **Azure Databricks**, and configuring **Databricks Secret Scopes**.

It can be used in job interviews by adding **screenshots** in the marked sections.

---

## 1. Overview

The goal of this infrastructure is to enable **centralized and secure secret management** across cloud data components such as:

- Azure Data Factory (ADF)
- Azure Databricks
- Cloud development and infrastructure processes

Azure Key Vault provides:

- Secure storage for **keys, passwords, tokens, and secrets**
- Access control via permissions and identity (RBAC / Azure AD)
- Alignment with security best practices

Databricks consumes Key Vault secrets through **Databricks Secret Scopes**, enabling secure access to external services and Linked Services without hardcoding credentials.

> **Screenshot Placeholder:**  
> Add a screenshot of the Key Vault overview page in Azure Portal.

---

## 2. Azure Key Vault Setup

### 2.1 Create a Key Vault

When creating a Key Vault, define:

- Target **Resource Group** per environment (DEV / TEST / PROD)
- Azure **Region**
- **Pricing Tier** (commonly *Standard*)

> **Screenshot Placeholder:**  
> Key Vault creation (basics) + the final Key Vault resource overview.

---

### 2.2 Configure Permissions (RBAC / Access Policies)

Define who is allowed to:

- Read secrets
- Create / update / delete secrets
- Manage keys or certificates

Common RBAC roles:

- **Key Vault Secrets User** – read-only access
- **Key Vault Secrets Officer** – manage secrets
- **Key Vault Administrator** – full administrative access

> **Screenshot Placeholder:**  
> RBAC role assignments for Key Vault (who has what access).

---

### 2.3 Create Secrets

Typical secrets stored in Key Vault:

- Databricks access token
- Oracle / SQL Server credentials
- Azure AI Document Intelligence API key
- OpenAI API key
- On-premises connectivity credentials (if required)

> **Screenshot Placeholder:**  
> Secrets list in Key Vault (keys masked, no sensitive data visible).

---

## 3. Connecting Key Vault to Azure Services

### 3.1 Use Managed Identity

Each Azure component (ADF, Databricks, Function Apps, etc.) is granted access to Key Vault via **Managed Identity**.

Benefits:

- Prevents credentials from being stored in code
- Strong authentication through Azure AD
- Easier governance and auditing

> **Screenshot Placeholder:**  
> Managed Identity enabled on a service + its Key Vault RBAC assignment.

---

### 3.2 Azure Data Factory – Key Vault Linked Service (Optional)

In Azure Data Factory:

- Create a **Linked Service** of type **Azure Key Vault**
- Authenticate using the **ADF Managed Identity**

For other Linked Services, use:

- **Store credentials in Key Vault**

This prevents credentials from being embedded in pipeline or Linked Service definitions.

> **Screenshot Placeholder:**  
> ADF Key Vault Linked Service configuration (authentication via Managed Identity).

---

## 4. Databricks Secret Scopes

Databricks does not access Key Vault directly. Instead, it uses a **Secret Scope** that is backed by Azure Key Vault.

### 4.1 Create a Key Vault–Backed Secret Scope

Create a Secret Scope of type **Azure Key Vault–backed**, including:

- Scope name (e.g., `kv-scope-dev`)
- Key Vault reference
- Ensure the Databricks workspace supports the required plan (Standard / Premium)

> **Screenshot Placeholder:**  
> Scope creation or UI/CLI output showing the created scope (no secrets displayed).

---

### 4.2 Use Secrets Inside Notebooks

Example:

```python
api_key = dbutils.secrets.get(scope="kv-scope-dev", key="OpenAI-Key")
