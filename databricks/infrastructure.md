# Databricks Infrastructure Setup – DEV & PROD Environments

## Introduction

This document describes the **Databricks infrastructure design and setup** based on two fully separated environments: **DEV** and **PROD**.

The documentation covers:
- Resource Groups and Databricks Workspaces
- Storage Accounts and data layer structure (Bronze / Silver / Gold)
- Unity Catalog catalogs and schemas
- Credentials and External Locations
- Table creation patterns (Managed / External)

Designated placeholders for screenshots are included throughout the document and can be populated when presenting this setup in job interviews.

---

## 1. Environment Separation: Resource Groups & Workspaces

Two isolated environments were created:

- **RG-DEV** – used for development and testing
- **RG-PROD** – used for stable and controlled production workloads

Each Resource Group contains a dedicated Databricks Workspace:

- `databricks-ws-dev`
- `databricks-ws-prod`

This separation ensures:
- Isolation between development and production
- Controlled promotion of changes
- Reduced operational and security risks

> **Screenshot Placeholder:**  
> Azure Portal – Resource Groups and Databricks Workspaces (DEV vs PROD)

---

## 2. Storage Accounts per Environment

Each Databricks Workspace is connected to a dedicated Storage Account:

- **stdev** – data storage for DEV
- **stprod** – data storage for PROD

Within each Storage Account, containers were created to support the **Lakehouse layered architecture**:

- `bronze`
- `silver`
- `gold`

### Typical Structure

```text
stdev
 ├─ bronze
 ├─ silver
 └─ gold

stprod
 ├─ bronze
 ├─ silver
 └─ gold
