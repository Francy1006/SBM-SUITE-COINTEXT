# PROJECT_CONTEXT.md

> **Last updated:** 2026-07-30
>
> **Purpose**
>
> Persistent global project context for **SBM Suite**.
>
> Read together with:
>
> ```text
> SUITE_CONTEXT.md
> BUSINESS_CONTEXT.md
> QA_CONTEXT.md
> ```

## 1. Current objective

```text
Git Markdown contexts
→ SBM-AI-ASSISTANT
→ embeddings
→ Qdrant sbm_contexts
→ ChatGPT context package
→ reviewed context updates
```

Git remains the source of truth. Qdrant is only the semantic index.

## 2. Suite structure

```text
SBM-SUITE/
├── context/
│   ├── PROJECT_CONTEXT.md
│   ├── README.md
│   ├── SUITE_CONTEXT.md
│   ├── BUSINESS_CONTEXT.md
│   ├── QA_CONTEXT.md
│   ├── SYS_PROMPT.md
│   ├── input/
│   └── output/
├── DP-API/
├── SBM-API/
├── SBM-DB/
├── SBM-MANAGER/
└── sbm-ai-assistant/
```

Each active project may contain:

```text
<PROJECT>/context/
├── PROJECT_CONTEXT.md
├── QA_CONTEXT.md
└── DEPLOY_CONTEXT.md
```

## 3. Repository responsibilities

```text
DP-API
→ client-facing business API

SBM-API
→ internal platform API

SBM-DB
→ PostgreSQL, Flyway and DBML source of truth

SBM-MANAGER
→ enterprise frontend

sbm-ai-assistant
→ AI orchestration, embeddings, Qdrant, RAG and Tools

SBM-SUITE/context
→ global architecture, business, QA and workflow context
```

## 4. Current DP-API domain ownership

```text
Product  → products
Material → material
Service  → service
Catalog  → catalog
Ticket   → ticket
```

Current status:

```text
Product
→ implemented and validated as reference vertical

Material
→ separated into dedicated app

Service
→ next planned backend domain

Catalog
→ future stage

Ticket
→ future stage
```

## 5. Database ownership

`SBM-DB` is authoritative for:

- PostgreSQL schemas;
- tables and columns;
- constraints and foreign keys;
- indexes and triggers;
- functions and reference data;
- Flyway migrations;
- DBML.

Application repositories must not create business-schema migrations.

## 6. Current AI architecture

```text
Confluence
→ sbm-ai-assistant
→ Qdrant sbm_docs
→ Slack chatbot
```

```text
SBM-SUITE and project Markdown
→ sbm-ai-assistant
→ chunking
→ multilingual embeddings
→ Qdrant sbm_contexts
→ context package export
```

Collections:

```text
sbm_docs
→ Confluence documentation

sbm_contexts
→ suite and project contexts
```

## 7. Context deployment and upgrade workflow

Project scripts:

```text
scripts/context-deploy.sh
scripts/context-upgrade.sh
```

Export flow:

```text
Git working-tree evidence
→ context-deploy.sh
→ POST /contexts/export
→ index allowed contexts in Qdrant sbm_contexts
→ retrieve relevant global and project chunks through RAG
→ include complete update-authorized files
→ generate context-package.zip and parameterized SYS_PROMPT.md
→ user uploads both to ChatGPT
```

Upgrade flow:

```text
ChatGPT
→ context-upgrade.zip
→ SBM-SUITE/context/input
→ context-upgrade.sh
→ POST /contexts/upgrade
→ validate manifest, paths and SHA-256 hashes
→ create timestamped backup
→ atomically replace only authorized files
→ remove input ZIP only after complete success
```

Update-authorized files:

```text
SBM-SUITE/PROJECT_CONTEXT.md
SBM-SUITE/README.md
SBM-SUITE/context/SUITE_CONTEXT.md
SBM-SUITE/dp-api/context/PROJECT_CONTEXT.md
SBM-SUITE/dp-api/README.md
```

Protected files:

```text
QA_CONTEXT.md
BUSINESS_CONTEXT.md
DEPLOY_CONTEXT.md
SYS_PROMPT.md
```

## 8. Current context export endpoint

Implemented in `sbm-ai-assistant`:

```text
POST /contexts/export
```

Current behavior:

- strict allowlist;
- validated source paths;
- independent `source_path` and `archive_path`;
- deterministic UUID5 identifiers;
- idempotent indexing;
- unchanged sources skip embeddings;
- changed sources embed in batches;
- separate `sbm_contexts` collection;
- atomic ZIP creation;
- manifest generation;
- RAG retrieval from `sbm_contexts`;
- separate global and project retrieval scopes;
- export of relevant chunks through `retrieved-context.md`;
- inclusion of complete update-authorized files;
- Git change evidence in the package;
- no raw vectors exported;
- no secrets or `.env` files included.

Expected DP-API sources:

```text
SBM-SUITE/PROJECT_CONTEXT.md
SBM-SUITE/README.md
SBM-SUITE/context/SUITE_CONTEXT.md
SBM-SUITE/context/BUSINESS_CONTEXT.md
SBM-SUITE/context/QA_CONTEXT.md
SBM-SUITE/context/SYS_PROMPT.md
SBM-SUITE/dp-api/README.md
SBM-SUITE/dp-api/context/PROJECT_CONTEXT.md
SBM-SUITE/dp-api/context/QA_CONTEXT.md
SBM-SUITE/dp-api/context/DEPLOY_CONTEXT.md
```

This file restores:

```text
/suite/context/PROJECT_CONTEXT.md
```

## 9. Runtime topology

```text
DP-API
→ host 8081
→ container 8000

SBM-API
→ host 8082
→ container 8000

sbm-ai-assistant
→ host 8000

Qdrant
→ host 6333
```

Current mounts:

```text
../context
→ /suite/context

../DP-API
→ /suite/DP-API
```

## 10. Stable rules

1. Git Markdown is the source of truth.
2. Qdrant is only an index.
3. `sbm_docs` and `sbm_contexts` remain separate.
4. AI never writes directly to PostgreSQL.
5. AI must call canonical APIs.
6. DP-API owns client operations.
7. SBM-API owns internal platform operations.
8. SBM-DB owns physical schema changes.
9. Context workflows preserve paths and filenames.
10. Context replacement must be atomic.
11. Protected contexts cannot be modified by `context-deploy`.
12. No secrets may enter ZIP packages or Git.
13. No unrelated project changes.
14. No commits or push unless requested.
15. Work one validated step at a time.

## 11. Completed

- global context structure defined;
- `SUITE_CONTEXT.md` created;
- `BUSINESS_CONTEXT.md` created;
- global `QA_CONTEXT.md` created;
- `SYS_PROMPT.md` created;
- DP-API contexts created;
- DP-API `context-deploy.sh` created;
- `POST /contexts/export` implemented;
- `sbm_contexts` implemented;
- deterministic and idempotent indexing implemented;
- context ZIP and manifest generation implemented;
- embedding performance issue corrected;
- source/archive path separation corrected;
- RAG-based context retrieval implemented;
- complete update-authorized files added to the export package;
- `POST /contexts/upgrade` implemented with validation, backup, atomic replacement and rollback;
- DP-API context export and upgrade scripts added.

## 12. Pending

1. Restore this file at:
   ```text
   SBM-SUITE/context/PROJECT_CONTEXT.md
   ```
2. Re-run:
   ```bash
   DP-API/scripts/context-deploy.sh
   ```
3. Confirm:
   ```text
   HTTP 200
   indexed_source_count = 10
   errors = []
   ```
4. Upload generated ZIP and `SYS_PROMPT.md` to ChatGPT.
5. Implement reviewed ZIP import later.
6. Create `qa-context-update`.
7. Add Notion synchronization and roadmap automation.
8. Create contexts for remaining projects.

## 13. Required behavior

Before changes:

1. identify target project;
2. read applicable contexts;
3. inspect actual repository state;
4. verify database source when relevant;
5. report missing information;
6. propose scope.

During changes:

- preserve canonical ownership;
- avoid speculative refactors;
- avoid unauthorized migrations;
- do not modify protected contexts;
- do not modify unrelated projects;
- do not perform Git operations unless requested.

After changes:

- report files modified;
- report validation executed;
- report validation not executed;
- report database impact;
- report migration impact;
- report remaining risks;
- update only authorized contexts.
