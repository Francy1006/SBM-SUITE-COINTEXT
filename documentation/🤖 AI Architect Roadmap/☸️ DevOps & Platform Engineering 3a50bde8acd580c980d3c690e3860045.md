# ☸️ DevOps & Platform Engineering

> **Last updated:** 2026-07-31
>
> **Purpose:**
>
> Define the validated DevOps and Platform Engineering operating model for SBM Suite, including the current documentation and context workflows used by `dp-api`.
>
> **Source of truth:**
>
> `DP-API` Git repository, `SBM-SUITE/context`, project contexts, workflow scripts and supplied Git evidence.

## 1. Overview

SBM Suite uses repository-based automation to package project evidence, retrieve relevant context and documentation from Qdrant, and apply validated upgrades through separate context and documentation workflows.

The current `dp-api` change set affects project contexts, project-tree generation, QA checks and the four workflow scripts for context and documentation deployment and upgrade.

## 2. Scope

This page covers:

- Docker-based local execution;
- Git evidence collection;
- context deployment and upgrade;
- documentation deployment and upgrade;
- Qdrant collection separation;
- ZIP manifests and SHA-256 validation;
- backups, atomic replacement and rollback;
- current validation limitations.

It does not claim production deployment, Kubernetes adoption, cloud execution, completed QA, coverage or SonarQube results.

## 3. Current state

The supplied Git evidence identifies changes in:

- `context/PROJECT_CONTEXT.md`;
- `context/QA_CONTEXT.md`;
- `project-tree.sh`;
- `project-tree.txt`;
- `scripts/context-deploy.sh`;
- `scripts/context-upgrade.sh`;
- `scripts/documentation-deploy.sh`;
- `scripts/documentation-upgrade.sh`;
- `scripts/qa-check.sh`.

The documentation deployment completed for `dp-api` and generated `documentation-package.zip`. The package includes rendered `SYS_PROMPT.md`, protected `FORMAT_CONTEXT.md`, authorized documentation paths, retrieved context, retrieved documentation and Git evidence.

No QA results file was supplied. Therefore, test execution, coverage, static analysis, migrations and deployment completion remain unverified.

## 4. Core concepts

- **Git source of truth:** repository content and Git evidence define the current manual workflow state.
- **Context workflow:** maintains project and global context independently from documentation.
- **Documentation workflow:** maintains complete authorized Markdown documentation replacements.
- **Protected contracts:** `SYS_PROMPT.md` and `FORMAT_CONTEXT.md` guide generation but are not upgrade outputs.
- **Authorized paths:** upgrades may affect only existing files explicitly included in the package allowlist.
- **Atomic replacement:** validated files are backed up and replaced as one controlled operation.
- **Evidence-first generation:** unsupported claims are omitted rather than inferred.

## 5. Architecture or operating model

```text
Developer change
→ Git evidence collection
→ context-deploy or documentation-deploy
→ Qdrant retrieval
→ generated evidence package
→ LLM-generated upgrade ZIP
→ backend validation
→ timestamped backup
→ atomic replacement
→ rollback on failure
```

| Component | Project | Responsibility | Technology | Runtime | Owner | Status |
|---|---|---|---|---|---|---|
| Context deploy | `sbm-ai-assistant` / `SBM-SUITE` | Package context evidence and retrieve relevant context | Python, FastAPI, Qdrant, ZIP | Docker | SBM Suite | active |
| Context upgrade | `sbm-ai-assistant` / `SBM-SUITE` | Validate and apply authorized context section updates | Python, FastAPI | Docker | SBM Suite | active |
| Documentation deploy | `sbm-ai-assistant` / `SBM-SUITE` | Index documentation and build a documentation evidence package | Python, FastAPI, Qdrant, ZIP | Docker | SBM Suite | active |
| Documentation upgrade | `sbm-ai-assistant` / `SBM-SUITE` | Validate and replace complete authorized Markdown files | Python, FastAPI | Docker | SBM Suite | active |
| Context collection | Qdrant | Store active context chunks | `sbm_contexts` | Docker | SBM Suite | active |
| Documentation collection | Qdrant | Store active documentation chunks | `sbm_documentation` | Docker | SBM Suite | active |

| Source | Target | Contract | Data | Authentication | Purpose | Status |
|---|---|---|---|---|---|---|
| Git repository | Deploy workflow | Repository paths and workflow request | Diff, changed files, log, QA and tree evidence | Local workflow access | Build evidence package | active |
| Documentation deploy | Qdrant | Documentation indexing contract | Markdown chunks | Internal service access | Retrieve relevant documentation | active |
| Context deploy | Qdrant | Context indexing contract | Context chunks | Internal service access | Retrieve relevant context | active |
| Upgrade workflow | Documentation root | Manifest, hashes and authorized paths | Complete Markdown replacements | Local workflow access | Apply validated documentation updates | active |

| ADR ID | Decision | Status | Consequences | Projects | Reference |
|---|---|---|---|---|---|
| N/A | Keep context and documentation workflows separate | accepted | Prevents documentation upgrades from modifying context files and vice versa | SBM Suite | `FORMAT_CONTEXT.md` |
| N/A | Keep Git as the current primary documentation source of truth | accepted | Notion synchronization remains downstream and must not overwrite Git silently | SBM Suite | `FORMAT_CONTEXT.md` |

## 6. Components

| Service | Project | Container | Internal port | Host port | Network | Health check | Status |
|---|---|---|---:|---:|---|---|---|
| Documentation backend | `sbm-ai-assistant` | backend | N/A | N/A | Docker Compose network | Backend endpoint validation | active |
| Qdrant | `sbm-ai-assistant` | qdrant | N/A | N/A | Docker Compose network | Collection access | active |
| PostgreSQL | `SBM-DB` | postgres | 5432 | 5432 | SBM network | Container health check | active |

Exact backend and Qdrant ports are not established by the supplied package and remain `N/A` here.

## 7. Workflows

| Step | Component | Input | Action | Output | Validation |
|---:|---|---|---|---|---|
| 1 | Git evidence collector | Repository | Read changed files, diff, log, QA and optional project tree | Evidence files | Safe relative paths and file-size limits |
| 2 | Documentation indexer | Authorized Markdown | Split and index documentation | `sbm_documentation` chunks | At least one indexable source |
| 3 | Retrieval service | Change query | Retrieve documentation and context chunks | Retrieved evidence | Project and active-state filters |
| 4 | Documentation exporter | Contracts and evidence | Render the project prompt and build package | `documentation-package.zip` | No unresolved template token |
| 5 | LLM generation | Documentation package | Generate complete authorized replacements | `documentation-upgrade.zip` | Prompt and format contract |
| 6 | Upgrade validator | Upgrade ZIP | Validate paths, manifest, metadata, headings and hashes | Staged valid files | All global checks must pass |
| 7 | Backup and apply | Valid staged files | Backup existing files and replace atomically | Updated documentation | Rollback on replacement failure |

| Artifact | Workflow | Producer | Consumer | Path | Required | Description |
|---|---|---|---|---|---:|---|
| `documentation-package.zip` | documentation deploy | Backend exporter | LLM | `context/documentation/output/` | 1 | Documentation evidence and workflow contracts |
| `documentation-upgrade.zip` | documentation upgrade | LLM | Backend validator | `context/documentation/input/` | 1 | Complete authorized Markdown replacements |
| `context-package.zip` | context deploy | Backend exporter | LLM | `context/output/` | 1 | Context evidence package |
| `context-upgrade.zip` | context upgrade | LLM | Backend validator | `context/input/` | 1 | Authorized context section updates |

| Workflow | Qdrant collection | Source of truth | Generated package | Upgrade output |
|---|---|---|---|---|
| Context workflow | `sbm_contexts` | Git context files | `context-package.zip` | `context-upgrade.zip` |
| Documentation workflow | `sbm_documentation` | Git documentation files | `documentation-package.zip` | `documentation-upgrade.zip` |
| Confluence assistant knowledge | `sbm_docs` | Confluence | N/A | N/A |

## 8. Configuration

The documentation workflow requires configured absolute directories for:

- documentation source root;
- deployment output;
- upgrade input;
- upgrade backup;
- `SYS_PROMPT.md`;
- `FORMAT_CONTEXT.md`.

The deployment package must render every project-name placeholder before ZIP creation. It must contain no unresolved template tokens.

Secret values and credentials must remain outside documentation and generated ZIP contents.

## 9. Security

- Reject absolute paths, `..`, duplicate paths and symlinks.
- Reject encrypted or corrupt ZIP members.
- Restrict replacements to existing authorized Markdown files.
- Protect `SYS_PROMPT.md` and `FORMAT_CONTEXT.md` from automated documentation upgrade.
- Validate SHA-256 hashes against the exact final file bytes.
- Create a backup before replacement.
- Roll back already replaced files when an application failure occurs.
- Do not package credentials, tokens, `.env` files or secret values.

## 10. Validation

Validated by the supplied workflow response:

- project: `dp-api`;
- workflow: `documentation-deploy`;
- status: `completed`;
- indexed documentation sources: `24`;
- indexed chunks: `3262`;
- collection: `sbm_documentation`;
- deployment errors: none reported.

The upgrade validator additionally requires exact metadata labels, exact level-two heading order, authorized paths, matching manifest lists and valid SHA-256 hashes.

QA evidence not supplied.

## 11. Known limitations

- No executed QA report was included.
- No coverage or SonarQube result was included.
- No production deployment evidence was included.
- Exact runtime ports for the documentation backend and Qdrant were not established by this package.
- Notion synchronization remains planned downstream work.
- The workflow remains manually initiated.

## 12. Roadmap

- Add reproducible QA execution evidence to documentation deployment packages.
- Add validated coverage and static-analysis results.
- Implement asynchronous workflow orchestration when approved.
- Add downstream Notion synchronization without changing Git ownership.
- Expand observability for indexing, retrieval, validation, backup and rollback.
- Maintain separate context, documentation and Confluence collections.

## 13. Related pages

| Page | Path | Relationship |
|---|---|---|
| AI Engineering | `SBM-SUITE/context/documentation/🤖 AI Architect Roadmap/🤖AI-Engineering 3a50bde8acd580fd84dbce95d8244e8d.md` | Defines AI, RAG and Tool architecture |
| QA and Testing | `SBM-SUITE/context/documentation/🤖 AI Architect Roadmap/🧪QA & Testing 3a50bde8acd580028bb1ffa68930e538.md` | Defines QA strategy and evidence requirements |
| Security and DevSecOps | `SBM-SUITE/context/documentation/🤖 AI Architect Roadmap/🔐 Security & DevSecOps 3a30bde8acd580b3abddca1cf6ff5ec9.md` | Defines security controls and risk handling |
| Observability and Monitoring | `SBM-SUITE/context/documentation/🤖 AI Architect Roadmap/📊 Observability & Monitoring 3a30bde8acd580d99985d1f851394d0c.md` | Defines operational telemetry goals |

## 14. Subpages

| Subpage | Path | Description | Status |
|---|---|---|---|
| N/A | N/A | No authorized subpages are listed for this page in the supplied package. | active |

## 15. Document boundary

This page defines the current DevOps and documentation workflow operating model supported by supplied evidence. It does not certify production readiness, cloud deployment, Kubernetes adoption, successful QA execution, coverage, SonarQube, database migration execution or Notion synchronization.
