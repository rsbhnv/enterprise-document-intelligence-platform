# CI/CD for Azure Data Factory  
**Option 1 – Pre & Post Deployment Tasks**  
**DEV → TEST → PROD**

---

## 1. Introduction
This document describes the CI/CD methodology implemented for **Azure Data Factory (ADF)**,
based on Microsoft’s official recommended approach:

**Option 1 – Pre and Post Deployment with PowerShell Scripts**

The document covers:
- Environment architecture (3 environments – DEV, TEST, PROD)
- Git integration limited to the DEV environment
- Use of ARM Templates for cross-environment deployments
- Full CI/CD pipeline implemented in Azure DevOps
- Pre-Deployment Script
- Post-Deployment Script
- Release process for TEST and PROD environments

---

## 2. Environment Architecture
Three isolated environments were established, each deployed in a separate Resource Group:

- **RG-DEV**
- **RG-TEST**
- **RG-PROD**

Only the **DEV** environment is connected to Git (Azure DevOps Repository).  
The **TEST** and **PROD** environments are not Git-connected; updates are applied
exclusively through the Release Pipeline.

### 
![Environment Resource Groups](images/cicd_2.png)
---

## 3. DEV Environment – Git Enabled
All development activities take place in the DEV environment:

- Git integration enabled
- Development performed using Feature Branches (if applicable)
- At the end of development, a **Publish** action is triggered
- The Publish action automatically generates:


```text
/factory        → ADF configurations (pipelines, datasets, linked services, etc.)
/arm_template   → template.json + parameters.json

---


## 4. CI/CD Process Overview

### Continuous Integration (CI)
After publishing from the DEV environment, ARM templates are generated.

The CI pipeline performs the following steps:
- Template validation
- Artifact build
- Publishing the artifact to Azure DevOps

---

### Continuous Delivery (CD)
The release process consists of two stages:
1. Deployment to **TEST**
2. Deployment to **PROD**

Both stages are based on:
- ARM Template (`template.json`)
- ARM Parameters
- Pre-deployment PowerShell script
- ARM Deployment
- Post-deployment PowerShell script
