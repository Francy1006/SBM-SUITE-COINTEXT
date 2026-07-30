# SUITE_CONTEXT.md

> **Last updated:** 2026-07-29
>
> **Purpose**
>
> This document is the persistent global context for **SBM Suite**. It explains how the suite is structured, how its projects interact, which component owns each responsibility, how containers and shared infrastructure communicate, and where project-specific context must be consulted.
>
> This file is not a replacement for the context of each repository. Before modifying a specific project, always read its own `context/project_context.md` and any applicable local QA or deployment context.

---

## 1. SBM Suite overview

SBM Suite is a modular ERP and business platform designed to separate:

- client-facing business operations;
- internal platform administration;
- frontend interaction;
- database ownership;
- AI-assisted workflows;
- quality assurance and deployment responsibilities.

The suite is developed as multiple independent repositories that communicate through explicit APIs and shared infrastructure.

Primary direction:

```text
Client user
→ frontend or approved interaction channel
→ client-facing API
→ validated domain operation
→ PostgreSQL managed by Flyway
```

Internal platform direction:

```text
Internal SBM user
→ internal administration interface
→ internal API
→ platform operation
→ PostgreSQL managed by Flyway
```

AI-assisted direction:

```text
Client user
→ Slack / frontend / approved channel
→ SBM AI Assistant
→ explicit API Tool
→ responsible API
→ validated business operation
```

---

## 2. Context hierarchy

The global context structure is:

```text
SBM-SUITE/
├── project_context.md
├── context/
│   ├── suite_context.md
│   ├── business_context.md
│   └── qa_context.md
├── DP-API/
│   └── context/
│       ├── project_context.md
│       ├── qa_context.md
│       └── deploy_context.md
├── SBM-API/
│   └── context/
│       ├── project_context.md
│       ├── qa_context.md
│       └── deploy_context.md
├── SBM-MANAGER/
│   └── context/
│       ├── project_context.md
│       ├── qa_context.md
│       └── deploy_context.md
├── SBM-DB/
│   └── context/
│       ├── project_context.md
│       ├── qa_context.md
│       └── deploy_context.md
└── SBM-AI-ASSISTANT/
    └── context/
        ├── project_context.md
        ├── qa_context.md
        └── deploy_context.md
```

Context responsibility:

```text
SBM-SUITE/project_context.md
→ current global project status and active cross-suite objective

SBM-SUITE/context/suite_context.md
→ architecture, ownership, integration, containers, services and shared data

SBM-SUITE/context/business_context.md
→ brands, products, services, operational concepts and business rules

SBM-SUITE/context/qa_context.md
→ QA rules for transversal behavior involving one or more repositories

<project>/context/project_context.md
→ technical and historical context exclusive to that repository

<project>/context/qa_context.md
→ tests, quality rules, coverage and validation specific to that repository

<project>/context/deploy_context.md
→ instructions for keeping contexts and deployment documentation synchronized
```

Mandatory reading rule:

Before working inside a repository, read:

```text
../project_context.md
../context/suite_context.md
../context/business_context.md
../context/qa_context.md
./context/project_context.md
./context/qa_context.md
./context/deploy_context.md
```

Only read the files that exist and are relevant to the current task. Project-specific context overrides generic assumptions, but it must not contradict validated suite-level ownership rules without an explicit architectural decision.

---

## 3. Main repositories

### 3.1 `SBM-MANAGER`

Role:

- Main enterprise frontend.
- Current technology: Vue.js 3.
- Used by client and internal users according to the screen and permission model.
- Consumes APIs; it does not own business persistence.
- Must not reproduce backend business rules that belong to an API.

Primary flows:

```text
Client operation
→ SBM Manager
→ DP-API
```

```text
Internal platform operation
→ SBM Manager
→ SBM-API
```

Frontend migrations must occur only after the target backend contract is stable and validated.

### 3.2 `DP-API`

Role:

- Client-facing Django REST API.
- Owns normal Ditaly Pasta business operations available to authorized client users.
- Uses a hybrid architecture.
- Business-critical domains use Hexagonal Architecture.
- Django models map Flyway-owned PostgreSQL tables.

Canonical domain ownership currently defined:

```text
Product  → products app
Material → material app
Service  → service app
Catalog  → catalog app
Ticket   → ticket app
```

Client-facing examples:

- products;
- materials;
- services;
- catalogs;
- prices;
- providers;
- branches;
- agreements;
- tickets;
- client-scoped users, roles and permissions.

DP-API must not:

- provision tenants;
- create franchises;
- activate uncontracted modules;
- own global platform billing;
- bypass Flyway;
- write directly to another tenant's data;
- expose internal platform operations.

### 3.3 `SBM-API`

Role:

- Internal platform API.
- Reserved for critical, contractual and administrative SBM operations.

Examples:

- franchise and tenant creation;
- module activation;
- subscription and plan management;
- global configuration;
- schema provisioning;
- internal users;
- internal auditing and operational controls.

Normal client applications must not use SBM-API for ordinary brand operations.

### 3.4 `SBM-DB`

Role:

- Database source of truth.
- Owns PostgreSQL schemas and physical structure.
- Uses Flyway for versioned migrations.
- Maintains DBML as the high-level relational reference.

Owns:

- schemas;
- tables;
- columns;
- foreign keys;
- constraints;
- indexes;
- triggers;
- functions;
- seed/reference data;
- versioned database changes.

Application repositories must not independently create business-table migrations.

### 3.5 `SBM-AI-ASSISTANT`

Role:

- AI Agent Orchestrator for SBM Suite.
- Detects intent and calls explicit Tools.
- Uses APIs as authoritative domain boundaries.
- Does not write directly to PostgreSQL.
- Must preserve user identity, tenant context, permissions and auditability.

Target flow:

```text
User
→ Slack / SBM Manager / approved channel
→ SBM AI Assistant
→ Tool
→ DP-API or SBM-API according to ownership
→ structured result
```

Current known foundation:

- FastAPI;
- Docker Compose;
- Qdrant;
- Confluence ingestion;
- Slack integration;
- multilingual embeddings;
- LLM provider abstraction;
- future DP-API Tools.

---

## 4. Ownership model

The fundamental ownership rule is:

```text
Client business operation
→ DP-API

Platform, contractual or provisioning operation
→ SBM-API

Physical database structure
→ SBM-DB / Flyway

User interface
→ SBM-MANAGER

AI orchestration
→ SBM-AI-ASSISTANT
```

Physical table location does not determine API ownership by itself.

Ownership is determined by:

1. who is authorized to execute the operation;
2. which domain owns the business rule;
3. which API must validate and audit the action;
4. whether the operation is client-facing or platform-internal.

Example:

```text
Create Product
→ DP-API

Create Franchise
→ SBM-API
```

---

## 5. Application architecture

SBM Suite uses a hybrid architecture.

### 5.1 Layered architecture

Appropriate for simple CRUD-oriented modules:

```text
router
→ ViewSet/controller
→ serializer
→ model
→ PostgreSQL
```

### 5.2 Hexagonal architecture

Required for business-critical domains with complex workflows, audit, lifecycle, pricing, integrations or high change frequency:

```text
REST adapter
→ application use case
→ domain entity/policy
→ repository port
→ infrastructure adapter
→ PostgreSQL
```

Rules:

- HTTP concerns remain in presentation adapters.
- Business workflows remain in application/domain layers.
- Persistence details remain in infrastructure adapters.
- Repository ports must not depend on Django.
- Domain-specific logic must not be shared only to reduce duplication.
- Cross-project communication occurs through APIs or explicit contracts, not direct imports between repositories.

---

## 6. Containers and shared infrastructure

Known local runtime components:

```text
SBM-MANAGER
→ frontend container

DP-API
→ dp-core
→ internal port 8000
→ host port 8081

SBM-API
→ sbm-core
→ internal port 8000
→ host port 8082

SBM-DB
→ PostgreSQL container
→ Flyway migration container

SBM-AI-ASSISTANT
→ FastAPI container
→ Qdrant container

SonarQube
→ independent SonarQube container
→ independent PostgreSQL database
```

Shared Docker network:

```text
sbm-network
```

The same internal port may be reused across different containers. Host ports and container names must remain unique.

Expected local communication:

```text
SBM-MANAGER
→ DP-API:8081
→ SBM-API:8082

DP-API / SBM-API
→ PostgreSQL through sbm-network

SBM-AI-ASSISTANT
→ DP-API / SBM-API through explicit API clients
→ Qdrant for vector search
```

Never assume a container name, port or hostname is current without checking the corresponding project context and Compose file.

---

## 7. Database model

Relevant PostgreSQL schemas currently include:

```text
ditaly_pasta
sbm_business
public
```

General interpretation:

```text
ditaly_pasta
→ brand-specific operational and commercial data

sbm_business
→ shared platform entities, global references and internal concepts

public
→ PostgreSQL or shared technical objects when applicable
```

Known client-operational tables include concepts such as:

- product;
- material;
- service;
- catalog;
- item configuration;
- provider;
- price;
- price configuration;
- branch;
- platform;
- agreements;
- ticket.

Shared concepts may include:

- item types;
- item groups;
- item categories;
- measure units;
- package types;
- transport types;
- geographic references;
- banks;
- user and authorization references.

Before implementing or moving any domain:

```text
Django model
↔ current PostgreSQL schema
↔ current Flyway scripts
↔ current DBML
↔ related API implementation
```

The live PostgreSQL schema and current Flyway project take precedence over stale application assumptions.

---

## 8. Database ownership restrictions

Business tables are managed externally by Flyway.

Do not execute from application repositories:

```bash
python manage.py makemigrations
python manage.py migrate
```

unless a separate architecture decision explicitly authorizes a Django-owned table.

For Flyway-owned tables:

- Django models remain unmanaged mappings.
- Model changes do not modify PostgreSQL.
- Structural changes belong to SBM-DB.
- Read-only inspection is allowed.
- Database writes must occur only through normal validated business operations or separately authorized database work.
- DBML, Flyway and runtime schema must remain synchronized.

---

## 9. Cross-project integration rules

### 9.1 Frontend to API

```text
SBM-MANAGER
→ DP-API for client operations
→ SBM-API for internal platform operations
```

The frontend must not choose an API based only on where an endpoint already exists. It must follow canonical ownership.

### 9.2 AI to API

```text
SBM-AI-ASSISTANT
→ Tool
→ responsible API
```

The AI layer must not:

- access business tables directly;
- invent identifiers;
- bypass validation;
- duplicate pricing or lifecycle rules;
- use unrestricted credentials for client actions;
- transform an API rejection into a successful result.

### 9.3 API to database

```text
DP-API / SBM-API
→ unmanaged ORM or approved data access
→ PostgreSQL
```

The APIs consume the schema; they do not own Flyway migrations.

### 9.4 Cross-API interaction

Direct API-to-API communication is allowed only when explicitly required by a business workflow.

Avoid:

- circular dependencies;
- duplicated ownership;
- shared database writes that bypass the responsible domain;
- synchronous coupling without a documented reason.

Asynchronous orchestration may be introduced later for workflows that require decoupling, retries or durable processing.

---

## 10. Business brands and tenants

SBM Suite is designed to support multiple businesses or brands.

Current validated brand context:

```text
Ditaly Pasta
```

A brand or tenant may own:

- products;
- materials;
- services;
- prices;
- providers;
- branches;
- catalogs;
- agreements;
- tickets;
- operational users and permissions.

Platform-level ownership remains separate:

- franchise creation;
- tenant provisioning;
- contracted modules;
- subscription state;
- global services;
- platform configuration.

Detailed business definitions belong to:

```text
SBM-SUITE/context/business_context.md
```

---

## 11. Security and identity

Current known concerns across the suite include:

- two user concepts may coexist between Django authentication and business users;
- tenant isolation requires explicit enforcement;
- object-level permissions must be validated;
- environment secrets must remain outside Git;
- permissive CORS is development-only;
- internal and client credentials must be separated;
- AI-triggered actions require the same authorization as direct user actions.

Target request resolution:

```text
identity
→ tenant/franchise
→ contracted modules
→ role
→ permission
→ restriction
→ requested object
→ validated action
```

No component may bypass this chain for convenience.

---

## 12. QA ownership

QA is divided into global and project-specific contexts.

Global transversal QA:

```text
SBM-SUITE/context/qa_context.md
```

Use it for:

- flows crossing multiple repositories;
- frontend/API contract validation;
- API/database integration;
- cross-service authentication;
- suite-level smoke tests;
- shared quality gates;
- release acceptance across components.

Project-specific QA:

```text
<project>/context/qa_context.md
```

Use it for:

- unit tests;
- domain tests;
- repository tests;
- local coverage;
- local SonarQube configuration;
- project-specific acceptance criteria.

During implementation, project contexts may forbid running the complete QA workflow until development is finished. Follow the most specific applicable context.

---

## 13. Deployment and context synchronization

Each repository must contain:

```text
context/deploy_context.md
```

This file defines:

- which contexts must be read before deployment;
- which contexts must be updated after architectural or deployment changes;
- environment-specific deployment requirements;
- validation and rollback expectations;
- how repository documentation remains synchronized.

Context-update rule:

When a change affects only one repository:

```text
update local project_context
→ update local deploy_context when deployment behavior changed
→ update local qa_context when QA behavior changed
```

When a change affects multiple repositories or suite ownership:

```text
update affected local contexts
→ update SBM-SUITE/project_context.md
→ update suite_context.md when architecture or interaction changed
→ update business_context.md when business meaning changed
→ update global qa_context.md when transversal validation changed
```

---

## 14. Stable suite rules

1. Client operations use DP-API.
2. Internal platform operations use SBM-API.
3. SBM-MANAGER consumes APIs and does not own backend rules.
4. SBM-AI-ASSISTANT uses Tools and APIs, never direct database writes.
5. SBM-DB and Flyway own the physical business schema.
6. Every business capability has one canonical owner.
7. Product, Material, Service, Catalog and Ticket are independent domain apps.
8. Physical schema location does not determine API ownership.
9. Cross-project behavior must use explicit contracts.
10. No duplicated implementation may be removed until all consumers are migrated.
11. No app may generate migrations for Flyway-owned business tables.
12. Context files must be updated whenever ownership, architecture, integration, QA or deployment behavior changes.
13. Work must proceed one validated step at a time.
14. Secrets, credentials and tokens must never be committed.
15. README files describe the intended completed project; context files preserve actual status, history, pending work and restrictions.

---

## 15. Required verification before work

Before starting a task, determine:

```text
Which repository owns the change?
Which domain owns the rule?
Is the operation client-facing or platform-internal?
Which API contract is authoritative?
Which database schema and table are involved?
Is the current Flyway/DBML information available?
Which project contexts must be read?
Does the task affect one project or multiple projects?
Which QA context applies?
Which deployment context must be updated?
```

If ownership, schema or context is incomplete, stop and request the missing information instead of guessing.

---

## 16. Current known direction

The current architectural direction is:

```text
Product  → products app
Material → material app
Service  → service app
Catalog  → catalog app
Ticket   → ticket app
```

Product is the accepted reference vertical.

Material has been separated into its dedicated app.

Service is the next authorized backend domain, but implementation must begin only after inspecting the current database project, Flyway, DBML and live read-only PostgreSQL schema.

Catalog, Ticket and remaining domains will be addressed in separately authorized stages.

---

## 17. Document boundary

This document explains the suite as a system.

Do not use it as the sole source for modifying a repository.

Always combine it with:

```text
SBM-SUITE/project_context.md
SBM-SUITE/context/business_context.md
SBM-SUITE/context/qa_context.md
<project>/context/project_context.md
<project>/context/qa_context.md
<project>/context/deploy_context.md
```

When sources disagree, report the conflict and verify the current repositories and runtime before modifying code.
