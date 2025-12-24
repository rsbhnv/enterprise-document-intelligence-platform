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
```
This structure enforces a clear separation between **raw**, **refined**, and **analytics-ready** data.

> **Screenshot Placeholder:**  
> Azure Storage Account containers (Bronze / Silver / Gold)

---

## 3. Unity Catalog – Catalogs and Schemas

Separate Unity Catalog catalogs were created per environment and data layer:

- `dev_bronze`
- `dev_silver`
- `dev_gold`
- `prod_bronze`
- `prod_silver`
- `prod_gold`

Within each catalog, schemas are created based on project needs, for example:

- `raw`
- `curated`
- `analytics`

This design provides:

- Strong governance
- Fine-grained access control
- Clear ownership per layer and environment

> **Screenshot Placeholder:**  
> Unity Catalog – list of catalogs and schemas

---

## 4. Credentials & External Locations

To allow Databricks to securely access the Storage Accounts, the following steps were performed.

---

### 4.1 Create Credentials

Credentials were created using one of the following approaches:

- Azure role-based access (recommended)
- Storage access key (when required)

Permissions were scoped only to the required containers, following the **principle of least privilege**.

---

### 4.2 Create External Locations

External Locations were created and mapped to each data layer:

- `dev_bronze_loc` → `stdev/bronze`
- `dev_silver_loc` → `stdev/silver`
- `dev_gold_loc` → `stdev/gold`

The same pattern applies to the PROD environment.

---

### 4.3 Associate External Locations with Catalogs

Each External Location is explicitly assigned to the relevant Unity Catalog, ensuring:

- Controlled access
- Clear ownership
- Proper governance across environments

> **Screenshot Placeholder:**  
> Unity Catalog – External Locations and assigned credentials

---

## 5. Table Creation – Managed vs External Tables

Once External Locations are configured, tables can be created.

### Data Layer Responsibilities

**Bronze tables**
- Raw ingestion
- Minimal transformation

**Silver tables**
- Cleaned and standardized data
- Basic transformations applied

**Gold tables**
- Curated, analytics-ready datasets

### Example Table Definition

```sql
CREATE TABLE dev_bronze.raw_events
USING delta
LOCATION 'abfss://bronze@stdev/...';


```
Both **Managed Tables** and **External Tables** can be used, depending on governance and lifecycle requirements.


```
UPDATE dev_silver.cleaned_events
SET source_system = 'UNKNOWN'
WHERE source_system IS NULL;

UPDATE dev_silver.cleaned_events
SET source_system = ref.system_name
FROM dev_silver.system_reference ref
WHERE cleaned_events.event_id = ref.event_id;

MERGE INTO dev_silver.customers t
USING dev_bronze.customers_staging s
ON t.customer_id = s.customer_id

WHEN MATCHED THEN
  UPDATE SET
    t.name        = s.name,
    t.email       = s.email,
    t.country     = s.country

WHEN NOT MATCHED THEN
  INSERT *
;```
---

## 6. Architecture Summary

The final infrastructure architecture includes:

- Fully isolated **DEV** and **PROD** environments
- Dedicated Databricks Workspace per environment
- Separate Storage Accounts per environment
- **Bronze / Silver / Gold** data layers (Lakehouse architecture)
- Unity Catalog with environment- and layer-based catalogs
- Secure access via Credentials and External Locations
- Combination of Managed and External Delta tables

This setup provides a **scalable, secure, and enterprise-ready** foundation for data platforms built on Azure Databricks.

---

## Notes for Interview Presentation

- Use **masked screenshots only** (no subscription IDs, URLs, or sensitive names)
- Keep naming generic (**DEV / PROD**)
- Emphasize:
  - Environment isolation
  - Governance with Unity Catalog
  - Secure storage access
  - Lakehouse best practices

