# SBM Suite context

## Overview

This repository is the Git source of truth for SBM Suite's global context contracts, documentation contracts, and manual context/documentation workflows.

## Purpose

It keeps cross-project knowledge synchronized without duplicating each project's internal service, model, script, or implementation inventory.

## Architecture

Canonical suite layout:

```text
SBM-SUITE/
├── context/
│   ├── documentation/pages/
│   ├── input/
│   ├── output/
│   ├── backup/
│   ├── project-tree.sh
│   └── project-tree.txt
├── dp/DP-API/
└── sbm/
    ├── SBM-API/
    └── sbm-ai-assistant/
```

Container project roots mirror the brand hierarchy under `/suite/<brand>/<project>`.

## Requirements

- Git working tree access to the current suite.
- Project `.env.dev` defining `SBM_SUITE_ROOT` for deployment scripts.
- The runtime dependencies required by each project-owned workflow.

## Configuration

Global contracts are `FORMAT_CONTEXT.md` and `SYS_PROMPT.md`. Documentation-specific contracts are `documentation/FORMAT_CONTEXT.md` and `documentation/SYS_PROMPT.md`. Secret values and `.env` files must never be included in packages, manifests, contexts, or documentation.

## Installation

No standalone installation is required for the Markdown contracts. Project-owned scripts consume this repository from the suite root.

## Runtime

`context-deploy` reads `SBM_SUITE_ROOT`, refreshes `project-tree.txt`, gathers bounded evidence, and requests a RAG package. `context-upgrade` validates section patches before atomic replacement. Documentation uses a separate deploy/upgrade flow for existing authorized pages.

## Usage

Use `input/` and `output/` for the global context workflow. Documentation pages live only below `documentation/pages/<page>/`, with subpages below `documentation/pages/<page>/subpages/`.

Every successful context upgrade writes one backup to `backup/<timestamp>_<project>/`, including original files, `EXECUTIVE_README.md`, `COMMIT_MESSAGE.md`, and `BACKUP_MANIFEST.json`.

## API or interfaces

The context workflow indexes global and project contexts in `sbm_contexts`. Project requests send `project_root=/suite/<brand>/<project>` to the context export interface.

## Development

Keep this README suite-level. Update it only for structural, architectural, shared functional, or global workflow changes. Reusable component inventories belong in each project's README.

## Validation

Validate exact headings and tables, authorized targets, repository-relative paths, manifest/file agreement, SHA-256 hashes, backup contents, and absence of secrets before applying an upgrade.

## Security

Do not commit or package secrets, tokens, credentials, `.env` files, raw vectors, absolute local paths, or unauthorized project files.

## Known limitations

The workflows remain manually initiated. Documentation creation, deletion, rename, and structural moves require explicit manual contract updates.

## Related documentation

- `PROJECT_CONTEXT.md`
- `SUITE_CONTEXT.md`
- `FORMAT_CONTEXT.md`
- `SYS_PROMPT.md`
- `documentation/FORMAT_CONTEXT.md`
- `documentation/SYS_PROMPT.md`
