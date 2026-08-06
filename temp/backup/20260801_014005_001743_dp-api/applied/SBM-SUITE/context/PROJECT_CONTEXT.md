# PROJECT_CONTEXT.md

> **Last updated:** 2026-07-30
>
> **Purpose**
>
> Persistent global project context for **SBM Suite**.
>
> **Accuracy note**
>
> Git Markdown is the current source of truth. Qdrant is a semantic index. Planned objectives, QA results, documentation state and implemented behavior remain explicitly separated.

## 1. Executive summary

SBM Suite is evolving toward a governed context and documentation lifecycle based on Git, RAG, Qdrant, ChatGPT-reviewed section patches, validated upgrades, QA evidence and later Notion synchronization.

The current design separates:

- project and global contexts;
- QA execution from QA interpretation;
- context workflows from documentation workflows;
- `sbm_contexts` from `sbm_documentation`;
- planned objectives from implemented and validated work.

## 2. Suite purpose

SBM Suite groups business APIs, internal platform APIs, data ownership, frontend applications, AI orchestration, QA evidence and operational documentation under shared governance rules.

Primary responsibilities:

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
→ AI orchestration, embeddings, Qdrant, RAG, context and documentation processing

SBM-SUITE/context
→ global project, suite, business, QA, security, data and decision contexts
```

## 3. Current objectives

| ID | Project | Objective | Status | Priority | Target date | Branch | Documentation |
|---|---|---|---|---:|---|---|---|
| OBJ-CTX-001 | SBM-SUITE | Validate and stabilize the expanded context governance model, synchronized section patches and project-tree evidence | active | 5 |  | FEATURE-expands-context-governance | `context/documentation/AI Architect Roadmap/`, `context/documentation/SBM-Suite/` |
| OBJ-DOC-001 | SBM-SUITE | Implement the manual documentation deploy and upgrade workflow with dedicated RAG and Qdrant collection | pending | 4 |  | FEATURE-adds-documentation-workflow | `context/documentation/AI Architect Roadmap/`, `context/documentation/Roadmap/`, `context/documentation/SBM-Suite/` |
| DP-QA-001 | DP-API | Define and implement the complete QA procedure and synchronized project and global QA contexts | active | 5 |  | FEATURE-implements-qa-procedure | `context/documentation/QA & Testing/`, `context/documentation/Development Roadmap/` |

Rules:

- statuses are limited to `active` and `pending`;
- priorities use `0` to `5`;
- target date is optional;
- completed or discarded objectives are removed from this table;
- completed evidence belongs in `Completed work`;
- every project objective change must update this table and the project summary.

## 4. Projects and ownership

| Project | Ownership | Main responsibilities | Source of truth |
|---|---|---|---|
| DP-API | Client-facing business operations | Products, materials, services, catalogs, tickets, providers, pricing, branches and other client domains | Project code, project contexts and canonical APIs |
| SBM-API | Internal platform operations | Internal administration, franchise, fiscal, inventory, calculation, configuration and platform services | Project code and project contexts |
| SBM-DB | Physical data model | PostgreSQL schemas, constraints, triggers, functions, reference data, Flyway and DBML | Database repository |
| SBM-MANAGER | Enterprise frontend | Administrative and operational user interface | Frontend repository |
| sbm-ai-assistant | AI and knowledge orchestration | Embeddings, Qdrant, RAG, Slack, context export/upgrade and future documentation workflows | AI repository and indexed Git Markdown |
| SBM-SUITE/context | Global governance | Cross-project context, architecture, business, QA, security, data, decisions and workflow contracts | Git Markdown |

## 5. Project objective summaries

| Project | Purpose | Active objective | Pending objectives | Branch | Main context | QA context | Documentation |
|---|---|---|---|---|---|---|---|
| DP-API | Client-facing business API | Define and implement the complete QA procedure | Dedicated Service app; Material consumer migration; duplicate Product endpoint retirement | `FEATURE-implements-qa-procedure` | `DP-API/context/PROJECT_CONTEXT.md` | `DP-API/context/QA_CONTEXT.md` | `context/documentation/QA & Testing/`, `context/documentation/Development Roadmap/` |
| SBM-API | Internal platform API | Not defined | Not defined | N/A | `SBM-API/context/PROJECT_CONTEXT.md` | `SBM-API/context/QA_CONTEXT.md` | To be mapped |
| SBM-DB | Database source of truth | Not defined | Not defined | N/A | `SBM-DB/context/PROJECT_CONTEXT.md` | `SBM-DB/context/QA_CONTEXT.md` | To be mapped |
| SBM-MANAGER | Enterprise frontend | Not defined | Not defined | N/A | `SBM-MANAGER/context/PROJECT_CONTEXT.md` | `SBM-MANAGER/context/QA_CONTEXT.md` | To be mapped |
| sbm-ai-assistant | AI orchestration and RAG | Support expanded context governance and project-tree evidence | Add documentation export, upgrade and dedicated collection | `FEATURE-expands-context-governance` | `sbm-ai-assistant/context/PROJECT_CONTEXT.md` | `sbm-ai-assistant/context/QA_CONTEXT.md` | `context/documentation/AI Engineering/`, `context/documentation/SBM-Suite/` |
| SBM-SUITE | Global governance and orchestration | Implement expanded context governance | Implement documentation workflow | `FEATURE-expands-context-governance` | `context/PROJECT_CONTEXT.md` | `context/QA_CONTEXT.md` | `context/documentation/` |

## 6. Global architecture

Current context architecture:

```text
Git Markdown contexts
→ context-deploy.sh
→ sbm-ai-assistant
→ embeddings
→ Qdrant sbm_contexts
→ RAG context package
→ ChatGPT section patches
→ context-upgrade.sh
→ validated backup and atomic replacement
```

Planned documentation architecture:

```text
Git Markdown documentation
+ updated contexts
→ documentation-deploy.sh
→ sbm-ai-assistant
→ embeddings
→ Qdrant sbm_documentation
→ RAG documentation package
→ ChatGPT updated authorized documentation Markdown
→ documentation-upgrade.sh
→ documentation backup and replacement
→ later Notion synchronization
```

QA architecture:

```text
qa-check.sh
→ tests
→ coverage
→ SonarQube
→ qa-results.md and evidence
→ context-deploy.sh
→ project QA_CONTEXT patch
→ global QA_CONTEXT summary patch
```

## 7. Shared infrastructure

Current shared infrastructure includes:

| Component | Purpose | Current ownership |
|---|---|---|
| PostgreSQL | Business and platform data | SBM-DB |
| Flyway | Business-schema migrations | SBM-DB |
| Qdrant | Semantic indexes | sbm-ai-assistant |
| Docker Compose | Local runtime orchestration | Each project and suite infrastructure |
| Git | Primary source of truth and version history | All projects |
| SonarQube | Static analysis and quality evidence | QA workflow |

Qdrant collections:

```text
sbm_docs
→ Confluence and assistant knowledge

sbm_contexts
→ suite and project contexts

sbm_documentation
→ roadmap and documentation pages
```

`sbm_documentation` is planned and must remain separate from `sbm_contexts`.

## 8. Cross-project integrations

```text
DP-API
→ client-facing requests
→ canonical business operations

SBM-API
→ internal platform operations

DP-API
→ asynchronous orchestration toward SBM-API where ownership requires it

SBM-DB
→ physical schema and migration authority

sbm-ai-assistant
→ canonical APIs and indexed Markdown
→ never writes directly to PostgreSQL

SBM-SUITE/context
→ global synchronization of project objectives and QA summaries
```

Every project `PROJECT_CONTEXT.md` update must update this global context.

Every project `QA_CONTEXT.md` update must update the global `QA_CONTEXT.md` summary.

## 9. Context deployment and upgrade workflow

### Deployment

```text
qa-check.sh
→ execute tests, coverage and SonarQube

context-deploy.sh
→ clean context/input and context/output
→ execute project-tree.sh
→ collect Git and QA evidence
→ require project-tree.txt
→ index authorized contexts in sbm_contexts
→ retrieve relevant chunks
→ generate context-package.zip
→ write context-export-response.json
→ validate status=completed
→ copy parameterized SYS_PROMPT.md to context/output
```

Expected package evidence:

```text
FORMAT_CONTEXT.md
retrieved-context.md
change-summary.md
changed-files.txt
git-diff.patch
git-log.txt
qa-results.md
project-tree.txt
manifest.json
```

### ChatGPT review

```text
user uploads context-package.zip + SYS_PROMPT.md
→ optional additional literal objective
→ evidence or user-guided mode
→ ChatGPT returns context-upgrade.zip with section patches
```

### Upgrade

```text
context-upgrade.zip
→ context/input
→ context-upgrade.sh
→ require exactly one context-upgrade.zip
→ validate manifest, paths, hashes and patch structure
→ validate response workflow, project, updated files and backup path
→ create timestamped context backup
→ apply authorized section patches atomically
→ print proposed commit message
→ remove input only after complete success
```

## 10. Documentation deployment and upgrade workflow

Documentation structure:

```text
context/documentation/
├── FORMAT_CONTEXT.md
├── SYS_PROMPT.md
├── input/
├── output/
├── backup/
└── <page>/
    ├── <page>.md
    └── subpages/
        └── <subpage>.md
```

Manual workflow:

```text
validated context upgrade
→ user confirms git status
→ documentation-deploy.sh
→ RAG from current documentation and updated contexts
→ documentation-package.zip + documentation SYS_PROMPT.md
→ user uploads both to ChatGPT
→ ChatGPT returns documentation-upgrade.zip
→ documentation-upgrade.sh
→ validate authorized existing pages and format
→ create timestamped documentation backup
→ replace only authorized Markdown
→ print proposed commit message
```

Rules:

- Git is the primary source of truth during the first stage;
- documentation pages and subpages are modified only when authorized by the documentation format;
- main pages are first-class documents and maintain subpage links;
- automated creation, deletion, rename and structural changes are not allowed initially;
- structural changes require manual updates to the page, documentation format and documentation system prompt;
- context and documentation upgrades remain separate;
- later synchronization with Notion may become bidirectional.

## 11. Current implementation status

Verified current capabilities include:

- global and DP-API context files exist;
- `POST /contexts/export` exists in `sbm-ai-assistant`;
- deterministic and idempotent context indexing exists;
- `sbm_contexts` exists;
- RAG-based context retrieval exists;
- context ZIP and manifest generation exist;
- `POST /contexts/upgrade` exists;
- manifest, path and SHA-256 validation exist;
- timestamped backup and atomic replacement exist;
- DP-API context deployment and upgrade scripts exist;
- user-guided and evidence execution modes are defined.
- section-level patch output and application exist;
- project-tree generation and package evidence exist;
- context deploy stores and validates the export response;
- context upgrade validates the response and confirms input cleanup;
- `qa-check.sh` writes execution evidence to `context/qa-results.md`;

Planned or under modification:

- expanded context types;
- project-to-global synchronization;
- project QA-to-global QA synchronization;
- dedicated documentation workflow;
- `sbm_documentation` collection;
- documentation backup and upgrade scripts;
- Git-to-Notion synchronization.

## 12. Validated decisions

1. Git Markdown is the primary source of truth during the manual stage.
2. Qdrant is a semantic index, not an authoritative store.
3. Context and documentation use separate Qdrant collections.
4. QA execution is performed by `qa-check.sh`; QA interpretation occurs in the context workflow.
5. Project objective changes synchronize to the global project context.
6. Project QA changes synchronize to the global QA context.
7. Context and documentation upgrades use separate deploy and upgrade workflows.
8. ChatGPT generates section-level context patches rather than complete context documents.
9. Documentation upgrade initially modifies only existing authorized pages.
10. Branch names are assigned before development using `FEATURE`, `BUGFIX` or `HOTFIX` and a maximum four-word slug.
11. Commit metadata is returned by the upgrade command to support one final commit.
12. Future asynchronous processing may use database configuration flags.

## 13. Accepted risks and constraints

- The current workflow is manual.
- Context and documentation consistency depends on completing both workflows when required.
- RAG retrieval may omit required source sections; unsafe patches must then be omitted.
- Project contexts for remaining repositories may not yet exist or may be incomplete.
- Documentation format and page authorization are not yet implemented.
- Notion synchronization is not yet implemented.
- Database flags and asynchronous orchestration are deferred.
- Existing contexts may require structural migration to the new format before patch-based upgrades succeed.

## 14. Completed work

- initial global context structure defined;
- `SUITE_CONTEXT.md` created;
- `BUSINESS_CONTEXT.md` created;
- global `QA_CONTEXT.md` created;
- `SYS_PROMPT.md` created;
- DP-API contexts created;
- DP-API `context-deploy.sh` created;
- DP-API `context-upgrade.sh` created;
- `POST /contexts/export` implemented;
- `POST /contexts/upgrade` implemented;
- `sbm_contexts` implemented;
- deterministic and idempotent indexing implemented;
- context ZIP and manifest generation implemented;
- RAG-based context retrieval implemented;
- context backup, validation, atomic replacement and rollback implemented;
- evidence and user-guided execution modes defined;
- documentation exported to Git under `context/documentation/`;
- expanded `FORMAT_CONTEXT.md` contract defined.
- section-level patch validation and application implemented;
- project-tree generation integrated into context deployment;
- context export response persistence and completion validation implemented;
- context upgrade input and response validation strengthened;
- QA evidence file generation implemented in `qa-check.sh`;

## 15. Pending work

1. Complete validated project-to-global synchronization coverage.
2. Execute the DP-API QA procedure and store real test, coverage and SonarQube evidence.
3. Synchronize the detailed DP-API QA context with the global QA summary after evidence exists.
4. Create `SECURITY_CONTEXT.md`.
5. Create `DATA_CONTEXT.md`.
6. Create `DECISIONS_CONTEXT.md`.
7. Complete and validate the documentation deploy and upgrade workflow.
8. Create and validate `sbm_documentation`.
9. Map all documentation pages to relevant contexts.
10. Add later Git-to-Notion synchronization.
11. Add later asynchronous database-flag orchestration.

## 16. Required behavior

Before changes:

1. identify the target project;
2. read applicable contexts;
3. inspect actual repository state;
4. execute QA when required;
5. verify database ownership when relevant;
6. report missing information;
7. define or update objective and branch before implementation.

During changes:

- preserve canonical ownership;
- avoid speculative refactors;
- avoid unauthorized migrations;
- do not expose secrets;
- do not modify unrelated projects;
- synchronize project and global contexts when required;
- keep implemented, planned and validated states separate;
- use only authorized section patches;
- do not perform Git operations unless requested.

After changes:

- report files modified;
- report validation executed and not executed;
- report database and migration impact;
- report remaining risks;
- update required project and global contexts;
- run documentation workflow when objectives, architecture, structure, technology or roadmap state changed;
- print the proposed commit message from the final upgrade.

## 17. Historical decisions

Previous context exports included complete update-authorized Markdown files. The accepted target design replaces that behavior with RAG-selected evidence and section-level JSON patches to reduce tokens and avoid unsafe full-document replacement.

Previous QA and business contexts were protected from context upgrades. The accepted target design authorizes their controlled update when evidence and synchronization rules require it.

Documentation was previously external to the context lifecycle. It is now versioned in Git and will use a separate deploy and upgrade workflow before later Notion synchronization.

## 18. Related documentation

Current documentation root:

```text
SBM-SUITE/context/documentation/
```

Primary documentation groups currently relevant to the global roadmap include:

- `AI Architect Roadmap`;
- `Development Roadmap`;
- `SBM-Suite`;
- `QA & Testing`;
- `Security & DevSecOps`;
- `AI Engineering`;
- `DevOps`;
- `Observability`;
- `Cloud`;
- `Automation`;
- `Technologies` when available in the exported hierarchy.

Exact page and subpage paths must be maintained by the documentation format contract.

## 19. Document boundary

This file records global project state, ownership, active and pending objectives, cross-project workflows, validated decisions, risks and high-level summaries.

It does not replace:

- detailed project contexts;
- `SUITE_CONTEXT.md` technical inventories and endpoint contracts;
- `BUSINESS_CONTEXT.md` brand and business metrics;
- project or global `QA_CONTEXT.md` test inventories and evidence;
- `SECURITY_CONTEXT.md` security controls;
- `DATA_CONTEXT.md` data ownership and lifecycle;
- `DECISIONS_CONTEXT.md` detailed ADR records;
- deployment contexts;
- roadmap and Notion documentation pages.
