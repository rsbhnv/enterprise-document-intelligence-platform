# Azure Data Factory (ADF)

Azure Data Factory (ADF) serves as the **orchestration backbone**
of the Enterprise Document Intelligence Platform.

ADF is responsible for coordinating the end-to-end processing lifecycle,
while delegating execution logic to specialized processing layers.

---

## Core Responsibilities

ADF is used to:
- Trigger document ingestion
- Coordinate processing steps across services
- Handle retries and failure scenarios
- Manage dependencies between pipeline stages
- Orchestrate AI and data processing workflows

ADF focuses on **control flow and orchestration**,
not on complex transformation logic.

---

## ADF Documentation Structure

This folder is organized into the following sections:

### 🏗️ Infrastructure
Environment setup, integration runtimes, linked services,
storage integration, and core platform components.

➡️ [`infrastructure.md`](infrastructure.md)

---

### 🧩 Development
Pipeline design patterns, parameterization,
modular orchestration logic, and execution flow.

➡️ [`development.md`](development.md)

---

### 🚀 CI/CD & Environments
Source control integration, deployment strategy,
and environment promotion considerations.

➡️ [`cicd.md`](cicd.md)

---

## Architectural Positioning

ADF acts as the **central orchestrator** in the platform:

- It does **not** perform heavy data transformations
- It does **not** embed business logic
- It orchestrates and coordinates:
  - Databricks processing
  - AI service invocation
  - Storage and data movement
  - Monitoring and alerting triggers

This separation ensures scalability,
maintainability, and operational clarity.

---

## Related Architecture Documentation

For system-level context, see:
- [`../architecture/overview.md`](../architecture/overview.md)
- [`../architecture/data-flow.md`](../architecture/data-flow.md)

---

## Summary

Azure Data Factory provides:
- Centralized orchestration
- Reliable execution control
- Clear separation of concerns
- Enterprise-ready workflow coordination

This makes ADF a critical component
in scalable, production-grade document processing platforms.

