# DATA_CONTEXT.md

> **Last updated:** 2026-07-30
>
> **Purpose**
>
> Persistent transversal data context for **SBM Suite**. It defines data ownership, databases, schemas, core entities, relationships, contracts, classification, lifecycle, integrity, migrations, retention, backup, recovery and observability.
>
> **Accuracy note**
>
> Only verified schemas, entities, relationships, classifications and lifecycle rules may be recorded as implemented. Unknown values remain `N/A`.

## 1. Data architecture overview

SBM Suite uses PostgreSQL as the primary relational data platform.

Canonical ownership:

```text
Physical schema
→ SBM-DB

Versioned database changes
→ Flyway

Application access
→ DP-API or SBM-API

AI access
→ approved API Tools only

Vector search
→ Qdrant
```

Application repositories consume the schema but do not own business-schema migrations unless explicitly authorized.

## 2. Data ownership

| Data domain | Source of truth | Operational owner | Schema owner | Access path |
|---|---|---|---|---|
| Client business data | PostgreSQL | DP-API | SBM-DB | DP-API |
| Internal platform data | PostgreSQL | SBM-API | SBM-DB | SBM-API |
| Physical schema and migrations | Flyway / DBML | SBM-DB | SBM-DB | Flyway |
| Global contexts | Git Markdown | SBM-SUITE | SBM-SUITE | Context workflow |
| Project contexts | Git Markdown | Project owner | SBM-SUITE / project | Context workflow |
| Documentation | Git Markdown | SBM-SUITE | SBM-SUITE | Documentation workflow |
| Confluence knowledge | Confluence | Documentation owner | Confluence | SBM-AI-ASSISTANT |
| Vector indexes | Qdrant | SBM-AI-ASSISTANT | SBM-AI-ASSISTANT | Vector API |
| QA evidence | Generated artifacts | Project owner | Project owner | QA workflow |

## 3. Databases and schemas

| Database | Schema | Owner project | Brand | Purpose | Migration owner | Status |
|---|---|---|---|---|---|---|
| PostgreSQL | `ditaly_pasta` | SBM-DB | Ditaly Pasta | Brand operational and commercial data | Flyway | active |
| PostgreSQL | `sbm_business` | SBM-DB | SBM | Shared platform and business reference data | Flyway | active |
| PostgreSQL | `public` | SBM-DB | SBM | Shared technical objects where applicable | Flyway | active |
| PostgreSQL | N/A | SonarQube infrastructure | SBM | SonarQube persistence | SonarQube stack | active |
| Qdrant | `sbm_docs` | SBM-AI-ASSISTANT | SBM | Confluence documentation vectors | SBM-AI-ASSISTANT | active |
| Qdrant | `sbm_contexts` | SBM-AI-ASSISTANT | SBM | Global and project context vectors | SBM-AI-ASSISTANT | active |
| Qdrant | `sbm_documentation` | SBM-AI-ASSISTANT | SBM | Git documentation vectors | SBM-AI-ASSISTANT | planned |

## 4. Core entities

| Entity | Owner project | Schema | Brand | Description | Sensitive | Source of truth |
|---|---|---|---|---|---:|---|
| Product | DP-API | `ditaly_pasta` | Ditaly Pasta | Sellable business item | 0 | PostgreSQL |
| Material | DP-API | `ditaly_pasta` | Ditaly Pasta | Production or operational input | 0 | PostgreSQL |
| Service | DP-API | `ditaly_pasta` | Ditaly Pasta | Non-physical commercial offering | 0 | PostgreSQL |
| Catalog | DP-API | `ditaly_pasta` | Ditaly Pasta | Grouping and publication of offerings | 0 | PostgreSQL |
| Ticket | DP-API | `ditaly_pasta` | Ditaly Pasta | Operational or support request | 1 | PostgreSQL |
| Price | DP-API | `ditaly_pasta` | Ditaly Pasta | Monetary state and history | 0 | PostgreSQL |
| Provider | DP-API | `ditaly_pasta` | Ditaly Pasta | Supplier and related business data | 1 | PostgreSQL |
| Branch | DP-API | `ditaly_pasta` | Ditaly Pasta | Physical or operational location | 1 | PostgreSQL |
| Agreement | DP-API | `ditaly_pasta` | Ditaly Pasta | Commercial relationship | 1 | PostgreSQL |
| Franchise | SBM-API | `sbm_business` | SBM | Contractual business unit | 1 | PostgreSQL |
| Tenant | SBM-API | `sbm_business` | SBM | Isolated client scope | 1 | PostgreSQL |
| User | DP-API / SBM-API | N/A | SBM / client brand | Authenticated person or service identity | 1 | PostgreSQL / auth system |
| Role | DP-API / SBM-API | N/A | SBM / client brand | Permission grouping | 0 | PostgreSQL |
| Permission | DP-API / SBM-API | N/A | SBM / client brand | Authorized capability | 0 | PostgreSQL |
| Restriction | DP-API / SBM-API | N/A | SBM / client brand | Constraint on capability scope | 0 | PostgreSQL |
| Context document | SBM-SUITE | Git | SBM | Persistent technical or business context | 0 | Git |
| Documentation page | SBM-SUITE | Git | SBM | Persistent operational documentation | 0 | Git |
| Vector chunk | SBM-AI-ASSISTANT | Qdrant | SBM | Embedded fragment of an indexed document | depends on source | Rebuildable index |

Boolean rule:

```text
1 = contains or may contain sensitive data
0 = not normally sensitive
```

## 5. Entity relationships

Validated high-level relationships:

```text
Tenant or franchise
→ users
→ roles
→ permissions
→ restrictions
```

```text
Brand
→ products
→ materials
→ services
→ catalogs
→ prices
→ providers
→ branches
→ agreements
→ tickets
```

```text
Product / Material / Service
→ current Price
→ Price history
```

```text
Catalog
→ references Products, Services and applicable Materials
```

Rules:

- Do not infer foreign keys from names alone.
- Physical relationships must be verified against PostgreSQL, Flyway and DBML.
- Shared references do not transfer domain ownership.
- Tenant and brand relationships must preserve isolation.

## 6. Data flows

Client operation:

```text
Client user
→ SBM-MANAGER
→ DP-API
→ PostgreSQL
```

Internal platform operation:

```text
Internal SBM user
→ SBM-MANAGER
→ SBM-API
→ PostgreSQL
```

AI-assisted operation:

```text
Authorized user
→ SBM-AI-ASSISTANT
→ Tool
→ DP-API or SBM-API
→ PostgreSQL
```

Context flow:

```text
Git contexts
→ SBM-AI-ASSISTANT
→ embeddings
→ Qdrant sbm_contexts
→ RAG package
→ reviewed patches
→ Git contexts
```

Documentation flow:

```text
Git documentation
→ SBM-AI-ASSISTANT
→ embeddings
→ Qdrant sbm_documentation
→ documentation package
→ reviewed Markdown updates
→ Git documentation
```

Confluence flow:

```text
Confluence
→ SBM-AI-ASSISTANT
→ chunking
→ embeddings
→ Qdrant sbm_docs
```

## 7. Data contracts

Every public data contract must define:

- owning project;
- endpoint;
- HTTP method;
- request schema;
- response schema;
- authentication;
- authorization;
- tenant scope;
- nullable fields;
- identifiers;
- versioning behavior;
- error contract;
- audit behavior.

Any endpoint, method, request body or response change must update:

```text
SUITE_CONTEXT.md
project PROJECT_CONTEXT.md
project QA_CONTEXT.md
global QA_CONTEXT.md when applicable
```

## 8. Data classification

| Classification | Description | Examples | Required handling |
|---|---|---|---|
| public | Safe for public disclosure | Public documentation | Normal integrity controls |
| internal | Internal operational information | Contexts, project structure | Access-controlled |
| confidential | Client or business information | Products, prices, providers | Tenant-scoped access |
| restricted | Highly sensitive operational data | Credentials, audit, personal data | Least privilege and encryption |
| secret | Authentication or infrastructure secrets | Tokens, passwords, private keys | Never stored in Git or Qdrant |

## 9. Sensitive data

Sensitive data may include:

- names;
- email addresses;
- phone numbers;
- addresses;
- geographic data;
- banking information;
- user identifiers;
- roles and permissions;
- audit records;
- credentials and tokens;
- provider contacts;
- branch contacts;
- support ticket contents.

Rules:

1. Minimize collection.
2. Restrict access by tenant and role.
3. Exclude secrets from logs and embeddings.
4. Do not place unrestricted personal data in context files.
5. Protect exports and backups.
6. Record classification and owner.
7. Apply retention and deletion rules.
8. Use secure transport outside trusted local environments.

## 10. Data integrity

Required controls:

- primary and foreign keys;
- constraints;
- transactional writes;
- exact decimal handling;
- logical deletion where defined;
- version fields;
- confirmation state;
- audit metadata;
- idempotency;
- concurrency control;
- rollback on failure;
- schema compatibility validation.

Mandatory verification:

```text
PostgreSQL
↔ Flyway
↔ DBML
↔ application model
↔ serializer
↔ public API contract
```

## 11. Migration ownership

Canonical rules:

- SBM-DB owns physical database changes.
- Flyway owns versioned business-schema migrations.
- DBML represents the high-level relational design.
- Application repositories map existing business tables.
- Django migrations must not modify Flyway-owned business schemas.
- A project may own a migration only through an explicit architecture decision.
- Migration execution must be evidenced before being marked complete.

Migration change workflow:

```text
business or technical requirement
→ SBM-DB change
→ Flyway migration
→ DBML update
→ application mapping update
→ tests
→ context synchronization
```

## 12. Retention and deletion

Required distinctions:

```text
logical deletion
physical deletion
retention expiration
archive
backup retention
vector index deletion
```

Rules:

- physical deletion requires explicit authorization;
- logical deletion preserves auditability;
- deleted source documents must be removed or deactivated in vector indexes;
- context version retention must preserve at least the current and required historical versions;
- backup retention must be defined per environment;
- personal and sensitive data must not be retained indefinitely without business or legal justification.

Current exact retention periods:

```text
N/A
```

## 13. Backup and recovery

Required backup scopes:

- PostgreSQL databases;
- Flyway migrations;
- DBML;
- Git repositories;
- context files;
- documentation files;
- Qdrant collections when operationally required;
- QA evidence where required for audit.

Recovery principles:

1. Test restoration.
2. Preserve timestamps and versions.
3. Protect backup credentials.
4. Encrypt sensitive backups.
5. Validate integrity after recovery.
6. Document recovery point and recovery time targets.
7. Treat Qdrant as rebuildable when source documents remain available.

Current RPO and RTO:

```text
N/A
```

## 14. Data observability

Required observability fields where applicable:

- correlation ID;
- project;
- endpoint or job;
- user or service;
- tenant or brand;
- entity;
- action;
- timestamp;
- result;
- row or item count;
- validation errors;
- retry count;
- duration;
- source and destination;
- non-sensitive diagnostic details.

Data observability must not expose secrets or unrestricted personal data.

## 15. Data risks

| Risk ID | Domain | Description | Projects | Status | Evidence | Risk | Owner |
|---|---|---|---|---|---|---:|---|
| DATA-001 | Ownership | Application and database assumptions may diverge | DP-API, SBM-API, SBM-DB | open | Manual verification required | 5 | SBM-DB and API owners |
| DATA-002 | Tenant isolation | Queries may expose another tenant's data | DP-API, SBM-API, SBM-AI-ASSISTANT | open | Complete transversal evidence unavailable | 5 | API owners |
| DATA-003 | Migration control | Unauthorized Django migrations may alter Flyway-owned schemas | DP-API, SBM-API | open | Context rule only | 5 | API owners |
| DATA-004 | Legacy data | Shared or inconsistent historical records may violate current assumptions | DP-API, SBM-DB | open | Known legacy concern | 4 | DP-API / SBM-DB |
| DATA-005 | Vector data | Sensitive content may enter Qdrant without filtering | SBM-AI-ASSISTANT | open | Complete classification controls unavailable | 5 | SBM-AI-ASSISTANT |
| DATA-006 | Retention | Exact deletion and retention periods are undefined | All | open | No approved retention policy | 4 | SBM Suite |
| DATA-007 | Backup recovery | Recovery objectives and restoration tests are undefined | All | open | No complete evidence | 4 | Infrastructure owners |

## 16. Pending data work

1. Validate all current schemas against Flyway and DBML.
2. Complete entity ownership mapping.
3. Define authoritative identifiers.
4. Define tenant and brand columns per entity.
5. Define personal and sensitive data classification.
6. Define retention periods.
7. Define physical versus logical deletion rules.
8. Define backup RPO and RTO.
9. Test database restoration.
10. Define Qdrant deletion and reindex procedures.
11. Add data integrity and migration tests.
12. Define audit and correlation standards.
13. Add authoritative metrics endpoints for business counts.
14. Document complete sales and order data models.

## 17. Related documentation

Relevant documentation domains include:

- Data Architecture;
- Database;
- Development;
- Security and DevSecOps;
- QA and Testing;
- Observability;
- DevOps;
- Cloud;
- Roadmap;
- SBM Suite.

Paths must use:

```text
SBM-SUITE/context/documentation/<page>/<page>.md
SBM-SUITE/context/documentation/<page>/subpages/<subpage>.md
```

Specific page paths will be added after the documentation structure is finalized.

## 18. Document boundary

This file defines transversal data ownership, schemas, entities, flows, classification, integrity, lifecycle, migration ownership, backup and risks.

It does not replace:

- live PostgreSQL inspection;
- Flyway migrations;
- DBML;
- project models;
- API schemas;
- security implementation;
- raw backup configurations;
- legal retention policy;
- QA execution evidence;
- documentation page content.

Verified database and repository evidence always takes precedence over stale context.
