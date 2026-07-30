# EXECUTIVE_README.md

## Project
dp-api

## Workflow
context-upgrade

## Date
2026-07-30

## Affected module
contexts

## General objective
Establish the context export and upgrade lifecycle using Git evidence, embeddings, Qdrant RAG retrieval and safe file replacement.

## Main completed changes
- Added `scripts/context-deploy.sh`.
- Added `scripts/context-upgrade.sh`.
- Consolidated the project context under `context/`.
- Documented RAG export, complete authorized files, backup, atomic replacement and rollback.

## Suite-level impact
SBM Suite now has a documented export → ChatGPT → upgrade lifecycle. Qdrant remains the semantic index and Git Markdown remains the source of truth.

## Validated evidence
QA evidence was not supplied and Git history was unavailable. The current iteration is not represented as fully validated.

## Database or migration impact
No database or migration changes were evidenced.

## Accepted risks
Untracked file contents may not be fully represented by the Git patch. Documentation was limited to behavior supported by the supplied package and existing contexts.

## Main pending work
- Validate a complete upgrade containing real context replacements.
- Integrate the formal QA evidence workflow.
- Review `git diff` before commit.

## Updated files
- `SBM-SUITE/PROJECT_CONTEXT.md`
- `SBM-SUITE/context/SUITE_CONTEXT.md`
- `SBM-SUITE/dp-api/context/PROJECT_CONTEXT.md`
- `SBM-SUITE/dp-api/README.md`
- `EXECUTIVE_README.md`
- `COMMIT_MESSAGE.md`
- `manifest.json`

## Proposed commit
`feat(contexts): add RAG context lifecycle`
