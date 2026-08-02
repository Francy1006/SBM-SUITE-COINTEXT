# SUITE_CONTEXT.md

> **Last updated:** 2026-07-30
>
> **Purpose**
>
> Persistent global technical context for **SBM Suite**. It defines brands, projects, applications, technologies, APIs, endpoint contracts, integrations, infrastructure and shared operational boundaries.
>
> **Accuracy note**
>
> Verified repository, runtime, API, database and QA evidence takes precedence over this document when a conflict exists. Unknown versions, bodies or response contracts must remain `N/A` until evidenced.

## 1. Suite identity

SBM Suite is a modular ERP and business platform composed of independent repositories with explicit ownership boundaries.

Primary responsibility model:

```text
Client business operations
→ DP-API

Internal platform operations
→ SBM-API

Frontend interaction
→ SBM-MANAGER

Physical database structure
→ SBM-DB / Flyway

AI orchestration
→ SBM-AI-ASSISTANT
```

Git Markdown is the current source of truth for contexts and documentation. Qdrant is a semantic index only.

## 2. Product scope

SBM Suite separates:

- client-facing business operations;
- internal platform administration;
- enterprise frontend interaction;
- database ownership and migrations;
- AI-assisted workflows;
- quality assurance;
- deployment;
- context and documentation lifecycle management.

Current business scope includes products, materials, services, catalogs, pricing, providers, branches, agreements, tickets, users, roles and permissions.

## 3. Brands and platforms

| Brand | Platform | Description | Status |
|---|---|---|---|
| SBM | SBM Suite | Platform, infrastructure, internal services and shared capabilities | active |
| Ditaly Pasta | Client ERP | Brand-specific operational and commercial platform | active |

`SBM` is treated as its own brand for technical, platform and infrastructure records.

## 4. Project map

| Project | Brand | Primary responsibility | Canonical owner |
|---|---|---|---|
| SBM-MANAGER | SBM | Enterprise frontend | Frontend interaction |
| DP-API | Ditaly Pasta | Client-facing business API | Client operations |
| SBM-API | SBM | Internal platform API | Platform administration |
| SBM-DB | SBM | PostgreSQL schemas, Flyway and DBML | Physical database structure |
| SBM-AI-ASSISTANT | SBM | AI orchestration, RAG, embeddings and Tools | AI-assisted workflows |
| SBM-SUITE/context | SBM | Global context and documentation contracts | Context governance |

## 5. Applications and services

| Brand | Project | Application or service | Type | Description | Language | Framework | Version | Runtime | Owner |
|---|---|---|---|---|---|---|---|---|---|
| SBM | SBM-MANAGER | Enterprise frontend | frontend | Client and internal user interface | JavaScript / TypeScript | Vue.js | 3 | browser / container | SBM-MANAGER |
| Ditaly Pasta | DP-API | Client-facing API | API | Business operations for authorized client users | Python | Django REST Framework | N/A | container | DP-API |
| SBM | SBM-API | Internal platform API | API | Critical, contractual and administrative platform operations | Python | Django REST Framework | N/A | container | SBM-API |
| SBM | SBM-DB | PostgreSQL | database | Business and platform persistence | SQL | PostgreSQL | 16 | container | SBM-DB |
| SBM | SBM-DB | Flyway | migration service | Versioned database migrations | SQL | Flyway | 10 | container | SBM-DB |
| SBM | SBM-AI-ASSISTANT | AI orchestrator | API / AI service | Intent routing, RAG, embeddings and explicit Tools | Python | FastAPI | N/A | container | SBM-AI-ASSISTANT |
| SBM | SBM-AI-ASSISTANT | Qdrant | vector database | Semantic indexes for documents, contexts and documentation | Rust service | Qdrant | N/A | container | SBM-AI-ASSISTANT |
| SBM | QA infrastructure | SonarQube | quality service | Static analysis and quality gates | Java service | SonarQube | N/A | container | QA workflow |

## 6. Technology inventory

| Brand | Project | Category | Technology | Version | Purpose | Status |
|---|---|---|---|---|---|---|
| SBM | SBM-MANAGER | frontend | Vue.js | 3 | Enterprise frontend | active |
| Ditaly Pasta | DP-API | backend | Python | N/A | API implementation | active |
| Ditaly Pasta | DP-API | backend framework | Django REST Framework | N/A | REST API | active |
| SBM | SBM-API | backend | Python | N/A | Internal API implementation | active |
| SBM | SBM-API | backend framework | Django REST Framework | N/A | Internal REST API | active |
| SBM | SBM-DB | database | PostgreSQL | 16 | Relational persistence | active |
| SBM | SBM-DB | migrations | Flyway | 10 | Schema versioning | active |
| SBM | SBM-AI-ASSISTANT | backend | Python | N/A | AI orchestration | active |
| SBM | SBM-AI-ASSISTANT | backend framework | FastAPI | N/A | AI API | active |
| SBM | SBM-AI-ASSISTANT | vector database | Qdrant | N/A | Semantic retrieval | active |
| SBM | Shared infrastructure | containers | Docker Compose | N/A | Local orchestration | active |
| SBM | QA infrastructure | static analysis | SonarQube | N/A | Quality gates | active |

## 7. Runtime architecture

Client-facing flow:

```text
Client user
→ SBM-MANAGER or approved channel
→ DP-API
→ validated domain operation
→ PostgreSQL
```

Internal platform flow:

```text
Internal SBM user
→ SBM-MANAGER
→ SBM-API
→ platform operation
→ PostgreSQL
```

AI-assisted flow:

```text
User
→ Slack / SBM-MANAGER / approved channel
→ SBM-AI-ASSISTANT
→ explicit Tool
→ DP-API or SBM-API
→ structured result
```

Context flow:

```text
Project Git evidence
→ context-deploy.sh
→ SBM-AI-ASSISTANT
→ Qdrant sbm_contexts
→ RAG package
→ ChatGPT
→ context-upgrade.zip
→ context-upgrade.sh
→ validated section patches
```

Documentation flow:

```text
Updated contexts + Git documentation
→ documentation-deploy.sh
→ Qdrant sbm_documentation
→ documentation package
→ ChatGPT
→ documentation-upgrade.zip
→ documentation-upgrade.sh
→ validated Markdown replacement
```

## 8. Data architecture

Relevant schemas currently include:

| Database | Schema | Owner project | Brand | Purpose | Migration owner | Status |
|---|---|---|---|---|---|---|
| PostgreSQL | ditaly_pasta | SBM-DB | Ditaly Pasta | Brand operational and commercial data | Flyway | active |
| PostgreSQL | sbm_business | SBM-DB | SBM | Shared platform and business references | Flyway | active |
| PostgreSQL | public | SBM-DB | SBM | Shared technical objects where applicable | Flyway | active |

Rules:

- SBM-DB and Flyway own business schema changes.
- Application models map existing tables.
- Application repositories must not generate business-schema migrations unless explicitly authorized.
- DBML, Flyway and runtime schema must remain synchronized.
- Detailed data ownership belongs in `DATA_CONTEXT.md`.

## 9. API inventory

| Brand | API | Owner project | Base path | Audience | Authentication | Description | Status |
|---|---|---|---|---|---|---|---|
| Ditaly Pasta | DP-API | DP-API | `/api` | Authorized client users | Required | Client-facing business operations | active |
| SBM | SBM-API | SBM-API | `/api` | Internal SBM users and services | Required | Internal platform administration | active |
| SBM | SBM-AI-ASSISTANT | SBM-AI-ASSISTANT | `/` | Approved channels and internal integrations | Endpoint-specific | AI orchestration and context services | active |

## 10. Endpoint contracts

| Brand | API | Method | Path | Request body | Response | Authentication | Purpose | Status |
|---|---|---|---|---|---|---|---|---|
| SBM | SBM-AI-ASSISTANT | GET | `/health` | none | health status | N/A | Service health check | implemented |
| SBM | SBM-AI-ASSISTANT | POST | `/contexts/export` | context export request | ZIP export metadata | Required by environment | Generate RAG context package | implemented |
| SBM | SBM-AI-ASSISTANT | POST | `/contexts/upgrade` | context upgrade ZIP | upgrade result and commit message | Required by environment | Validate and apply context patches | implemented |
| SBM | SBM-AI-ASSISTANT | POST | `/confluence/pages/{id}/ingest` | ingestion request | ingestion result | Required by environment | Ingest one Confluence page | implemented |
| SBM | SBM-AI-ASSISTANT | POST | `/confluence/ingest` | ingestion request | ingestion result | Required by environment | Ingest Confluence content | implemented |
| SBM | SBM-AI-ASSISTANT | POST | `/confluence/sync` | synchronization request | synchronization result | Required by environment | Synchronize Confluence content | implemented |
| SBM | SBM-AI-ASSISTANT | POST | `/slack/test` | Slack test payload | test result | Required by environment | Validate Slack integration | implemented |
| SBM | SBM-AI-ASSISTANT | POST | `/slack/rag` | Slack RAG query | assistant response | Slack validation | Execute RAG response | implemented |
| SBM | SBM-AI-ASSISTANT | POST | `/slack/events` | Slack event body | acknowledgement / response | Slack signature | Receive Slack events | implemented |
| Ditaly Pasta | DP-API | GET | `/api/products` | none | product collection | Required | List products | implemented |
| Ditaly Pasta | DP-API | POST | `/api/products` | product payload | created product | Required | Create product | implemented |
| Ditaly Pasta | DP-API | GET | `/api/products/{id}` | none | product detail | Required | Retrieve product | planned |
| Ditaly Pasta | DP-API | PATCH | `/api/products/{id}` | partial product payload | updated product | Required | Update product | planned |
| Ditaly Pasta | DP-API | DELETE | `/api/products/{id}` | none | deletion result | Required | Soft-delete product | planned |

Any endpoint creation, removal, path change, method change, request body change or response change must update this table.

## 11. Authentication and authorization

Cross-suite resolution target:

```text
identity
→ tenant or franchise
→ contracted modules
→ role
→ permission
→ restriction
→ requested object
→ validated action
```

Rules:

- Client and internal credentials remain separated.
- Tenant isolation requires explicit enforcement.
- Object-level permissions must be validated.
- AI-triggered actions require the same authorization as direct user actions.
- Internal platform operations must not be exposed through DP-API.
- Detailed security controls belong in `SECURITY_CONTEXT.md`.

## 12. Integrations and data flows

| Source | Target | Contract | Purpose | Status |
|---|---|---|---|---|
| SBM-MANAGER | DP-API | REST API | Client business operations | active |
| SBM-MANAGER | SBM-API | REST API | Internal platform operations | active |
| SBM-AI-ASSISTANT | DP-API | Explicit Tool / REST API | AI-assisted client operations | planned |
| SBM-AI-ASSISTANT | SBM-API | Explicit Tool / REST API | AI-assisted internal operations | planned |
| DP-API | PostgreSQL | ORM / approved data access | Business persistence | active |
| SBM-API | PostgreSQL | ORM / approved data access | Platform persistence | active |
| SBM-AI-ASSISTANT | Qdrant | Vector API | Semantic retrieval | active |
| SBM-AI-ASSISTANT | Confluence | REST API | Documentation ingestion | active |
| SBM-AI-ASSISTANT | Slack | Events API | Assistant interface | active |
| Context workflow | ChatGPT | ZIP + SYS_PROMPT | Reviewed context generation | active |
| Documentation workflow | ChatGPT | ZIP + SYS_PROMPT | Reviewed documentation generation | planned |

Cross-project communication must use explicit APIs or contracts. Direct repository imports and uncontrolled shared writes are prohibited.

## 13. Infrastructure and containers

| Component | Container or service | Internal port | Host port | Network | Status |
|---|---|---:|---:|---|---|
| DP-API | dp-core | 8000 | 8081 | sbm-network | active |
| SBM-API | sbm-core | 8000 | 8082 | sbm-network | active |
| SBM-AI-ASSISTANT | backend | 8000 | 8000 | sbm-network | active |
| Qdrant | qdrant | 6333 | 6333 | sbm-network | active |
| PostgreSQL | postgres | 5432 | 5432 | sbm-network | active |
| Flyway | flyway | N/A | N/A | sbm-network | active |
| SonarQube | sonarqube | N/A | N/A | independent/shared as configured | active |

Do not assume current names, ports or versions without checking the project Compose files.

## 14. Shared configuration

Shared configuration rules:

- secrets and `.env` values must remain outside Git and ZIP packages;
- project-specific environment files own local runtime values;
- context packages may include metadata but never secret values;
- repository-relative paths are required in manifests and documentation references;
- context and documentation collections remain separated;
- project, brand, document type, path, version and content hash must be available as retrieval filters.

## 15. Context and knowledge architecture

Qdrant collections:

| Collection | Owner | Content | Purpose |
|---|---|---|---|
| `sbm_docs` | SBM-AI-ASSISTANT | Confluence documentation | Assistant knowledge |
| `sbm_contexts` | SBM-AI-ASSISTANT | Global and project contexts | Context RAG |
| `sbm_documentation` | SBM-AI-ASSISTANT | Git documentation pages | Documentation RAG |

Rules:

- collections remain separate;
- vectors are never exported;
- Git Markdown is the source of truth;
- Qdrant is a rebuildable semantic index;
- context packages use selected chunks, evidence and format contracts;
- documentation packages use documentation chunks and updated contexts;
- raw complete source files should not be exported when section-level patches are sufficient.

## 16. Deployment model

Current stage:

```text
manual validated workflow
```

Future stage:

```text
database configuration flags
→ asynchronous orchestration
→ automatic context and documentation processing
```

Current deployment principles:

- one validated step at a time;
- Docker-based local services;
- backup before replacement;
- manifest and hash validation;
- atomic application or rollback;
- no automatic Git commit or push unless requested.

## 17. Security rules

1. Secrets, credentials and tokens must never enter Git or generated ZIP files.
2. AI services must not write directly to business databases.
3. User identity, tenant and authorization context must be preserved.
4. API ownership must not be bypassed.
5. Internal and client operations remain separated.
6. CORS and development credentials must not be treated as production configuration.
7. Input ZIP paths, hashes and symlinks must be validated.
8. Context and documentation replacements require backups.
9. Detailed controls and risks belong in `SECURITY_CONTEXT.md`.

## 18. Operational constraints

- The workflow remains manual in the first stage.
- Context changes precede documentation changes.
- Documentation deploy uses updated contexts.
- Context and documentation upgrades produce one final commit message each.
- `git status` confirms changed files but not semantic correctness.
- Structural format changes require manual updates to format contracts.
- Unknown facts, versions, endpoint bodies and QA results must remain `N/A` or unchanged.
- No unrelated project files may be modified.

## 19. Current suite state

Current verified direction:

- Product is the accepted DP-API reference vertical.
- Material is separated into its own domain app.
- Service is a planned backend domain.
- Catalog and Ticket remain future domains.
- Context export and upgrade endpoints exist.
- Context RAG uses `sbm_contexts`.
- Context output is being migrated from complete documents to section-level patches.
- Global Project, Suite, Business and QA contexts exist.
- Security, Data and Decisions contexts are being introduced.
- Documentation lifecycle and `sbm_documentation` are planned.
- `project-tree.txt` is planned as structural evidence for context deployment.

## 20. Context deployment lifecycle

```text
1. qa-check.sh
2. execute tests, coverage and SonarQube
3. context-deploy.sh
4. clean context input and output
5. generate project-tree.txt
6. index contexts in sbm_contexts
7. retrieve relevant chunks
8. generate context-package.zip and SYS_PROMPT.md
9. user uploads files to ChatGPT with or without additional prompt
10. ChatGPT returns context-upgrade.zip
11. user places ZIP in context/input
12. context-upgrade.sh validates patches
13. create timestamped context backup
14. apply synchronized context patches atomically
15. return commit message in terminal
16. user reviews git status
```

Required synchronized updates:

- project `PROJECT_CONTEXT.md` → global `PROJECT_CONTEXT.md`;
- project `QA_CONTEXT.md` → global `QA_CONTEXT.md`;
- structural/API/body/technology changes → `SUITE_CONTEXT.md`;
- brand or business capability changes → `BUSINESS_CONTEXT.md`;
- security changes → `SECURITY_CONTEXT.md`;
- data changes → `DATA_CONTEXT.md`;
- architecture and product decisions → `DECISIONS_CONTEXT.md`.

## 21. Documentation lifecycle

```text
1. complete context upgrade
2. review context changes
3. documentation-deploy.sh
4. index documentation in sbm_documentation
5. retrieve relevant documentation and updated context chunks
6. generate documentation-package.zip and documentation SYS_PROMPT.md
7. user uploads files to ChatGPT with or without additional prompt
8. ChatGPT returns documentation-upgrade.zip
9. user places ZIP in documentation/input
10. documentation-upgrade.sh validates authorized Markdown files
11. create timestamped documentation backup
12. replace validated documentation files
13. return commit message in terminal
14. user reviews git status
```

Current rules:

- Git is the primary source of truth.
- Only existing pages and subpages authorized by documentation format may be modified.
- Creation, deletion, rename or structural change requires manual format and prompt updates.
- Main pages are documents and must maintain subpage links.
- Notion synchronization is downstream and planned for a later stage.

## 22. Related documentation

Documentation paths follow:

```text
SBM-SUITE/context/documentation/<page>/<page>.md
SBM-SUITE/context/documentation/<page>/subpages/<subpage>.md
```

Relevant documentation domains include:

- SBM Suite;
- Development;
- Roadmap;
- Technologies;
- Security and DevSecOps;
- QA and Testing;
- Observability;
- DevOps;
- Cloud;
- Automation;
- AI Engineering.

Specific paths must be recorded in project objectives and context references when the documentation tree is finalized.

## 23. Document boundary

This document defines the suite as a technical system.

It does not replace:

- project-specific `PROJECT_CONTEXT.md`;
- detailed project QA plans;
- global or project security evidence;
- data governance details;
- business metrics;
- ADR history;
- deployment instructions;
- documentation page content;
- live repository, runtime, API or database evidence.

When sources disagree, report the conflict and verify the current repositories and runtime before modifying code or context.
