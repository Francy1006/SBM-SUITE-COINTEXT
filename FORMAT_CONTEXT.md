# FORMAT_CONTEXT.md

> **Purpose**
>
> Canonical structure contract for every SBM Suite context file.
> Context generation and upgrade processes must preserve these formats exactly.

## 1. Global rules

1. Preserve the exact heading names and order defined here.
2. Do not rename, merge, split, reorder or remove required sections.
3. Add content only inside the matching section.
4. Preserve the metadata block at the beginning of each file.
5. Preserve Markdown lists, tables, code blocks and path formatting.
6. Do not duplicate information across sections.
7. Do not create unsupported facts, tests, migrations, deployments or decisions.
8. When evidence is insufficient, keep the existing content unchanged.
9. A structural change requires an explicit update to this file.
10. If a complete source file is unavailable, do not generate a replacement.
11. Protected context files remain read-only unless their workflow explicitly allows modification.
12. All dates use `YYYY-MM-DD`.

---

## 2. Global `PROJECT_CONTEXT.md`

Required structure:

```text
# PROJECT_CONTEXT.md

> Last updated
> Purpose
> Accuracy note

## 1. Executive summary
## 2. Current suite objective
## 3. Projects and ownership
## 4. Global architecture
## 5. Shared infrastructure
## 6. Cross-project integrations
## 7. Context deployment and upgrade workflow
## 8. Current implementation status
## 9. Validated decisions
## 10. Accepted risks and constraints
## 11. Completed work
## 12. Pending work
## 13. Required behavior
## 14. Historical decisions
## 15. Document boundary
```

Section rules:

- `Executive summary`: concise suite state.
- `Current suite objective`: active global objective only.
- `Projects and ownership`: project responsibilities and boundaries.
- `Global architecture`: suite-level architecture only.
- `Shared infrastructure`: shared databases, networks, containers and services.
- `Cross-project integrations`: contracts and data flows between projects.
- `Context deployment and upgrade workflow`: context lifecycle.
- `Current implementation status`: current verified state.
- `Validated decisions`: accepted architectural and product decisions.
- `Accepted risks and constraints`: known limitations.
- `Completed work`: completed suite-level milestones.
- `Pending work`: transversal pending work.
- `Required behavior`: mandatory operating rules.
- `Historical decisions`: relevant superseded or historical decisions.
- `Document boundary`: information intentionally excluded.

---

## 3. Global `SUITE_CONTEXT.md`

Required structure:

```text
# SUITE_CONTEXT.md

> Last updated
> Purpose
> Accuracy note

## 1. Suite identity
## 2. Product scope
## 3. Project map
## 4. Ownership boundaries
## 5. Runtime architecture
## 6. Data architecture
## 7. API boundaries
## 8. Authentication and authorization
## 9. Integrations
## 10. Infrastructure and containers
## 11. Shared configuration
## 12. Context and knowledge architecture
## 13. Deployment model
## 14. Security rules
## 15. Operational constraints
## 16. Current suite state
## 17. Context deployment lifecycle
## 18. Document boundary
```

Section rules:

- Describe only suite-wide behavior.
- Do not include project implementation transcripts.
- Do not duplicate complete project contexts.
- Record ownership, boundaries and shared flows.

---

## 4. Global `BUSINESS_CONTEXT.md`

Required structure:

```text
# BUSINESS_CONTEXT.md

> Last updated
> Purpose
> Accuracy note

## 1. Business overview
## 2. Product vision
## 3. Business actors
## 4. Organizations and brands
## 5. Core business domains
## 6. Business entities
## 7. Business rules
## 8. Commercial flows
## 9. Pricing and fiscal concepts
## 10. Inventory and catalog concepts
## 11. Sales and order concepts
## 12. Provider and branch concepts
## 13. Terminology
## 14. Validated business decisions
## 15. Business constraints
## 16. Pending business definitions
## 17. Document boundary
```

Section rules:

- Store business meaning, not implementation detail.
- Technical references are allowed only when required to explain ownership.
- Do not infer business rules from code alone.

---

## 5. Global `QA_CONTEXT.md`

Required structure:

```text
# QA_CONTEXT.md

> Last updated
> Purpose
> Accuracy note

## 1. QA strategy
## 2. Quality gates
## 3. Test levels
## 4. Test environments
## 5. Required evidence
## 6. Coverage rules
## 7. Static analysis
## 8. Security validation
## 9. API validation
## 10. Database validation
## 11. Deployment validation
## 12. Defect classification
## 13. Release criteria
## 14. Accepted exceptions
## 15. Current QA status
## 16. Pending QA work
## 17. Document boundary
```

Section rules:

- Record only executed and evidenced validation.
- Never invent coverage, SonarQube, tests or deployments.
- Separate required QA policy from current QA results.

---

## 6. Global `SYS_PROMPT.md`

Required structure:

```text
# SYS_PROMPT.md

## Parameters
## Objective
## Required inputs
## Input meaning
## Change determination
## Allowed outputs
## Protected files
## Context format contract
## Context reconstruction rules
## Project context
## Suite project context
## README files
## QA evidence
## Commit nomenclature
## Executive summary
## Database rules
## Output rules
## Manifest
```

Section rules:

- `Context format contract` must require compliance with this file.
- The prompt must not redefine formats independently.
- Output filenames and manifest contracts must be explicit.
- Protected files must be listed explicitly.

---

## 7. Project `context/PROJECT_CONTEXT.md`

Required structure:

```text
# PROJECT_CONTEXT.md

> Last updated
> Purpose
> Accuracy note

## 1. Executive summary
## 2. Project purpose
## 3. Current objective
## 4. Scope and ownership
## 5. Architecture
## 6. Runtime and containers
## 7. Configuration
## 8. Modules
## 9. Data model ownership
## 10. API surface
## 11. Authentication and authorization
## 12. Integrations
## 13. Implemented behavior
## 14. Validation evidence
## 15. Database and migration impact
## 16. Security considerations
## 17. Accepted risks and constraints
## 18. Completed work
## 19. Pending work
## 20. Required behavior
## 21. Historical decisions
## 22. Document boundary
```

Section rules:

- Keep implementation state separate from planned work.
- Endpoint behavior belongs in `API surface`.
- Completed implementation belongs in `Implemented behavior`.
- Test results belong only in `Validation evidence`.
- Database impact must state explicitly when none exists.
- Project-specific headings may be added only through an approved update to this format file.

---

## 8. Project `context/QA_CONTEXT.md`

Required structure:

```text
# QA_CONTEXT.md

> Last updated
> Purpose
> Accuracy note

## 1. Project QA scope
## 2. Required quality gates
## 3. Test structure
## 4. Unit tests
## 5. Integration tests
## 6. API tests
## 7. Database tests
## 8. Security tests
## 9. Static analysis
## 10. Coverage
## 11. Test data and fixtures
## 12. Environment requirements
## 13. Current validated evidence
## 14. Known defects
## 15. Accepted exceptions
## 16. Pending QA work
## 17. Document boundary
```

Section rules:

- Distinguish policy from current results.
- Every result must include its evidence source.
- Do not overwrite historical evidence without preserving relevant records.

---

## 9. Project `context/DEPLOY_CONTEXT.md`

Required structure:

```text
# DEPLOY_CONTEXT.md

> Last updated
> Purpose
> Accuracy note

## 1. Deployment overview
## 2. Environments
## 3. Runtime topology
## 4. Containers and services
## 5. Networks and ports
## 6. Configuration and secrets
## 7. Build process
## 8. Deployment process
## 9. Database deployment
## 10. Health checks
## 11. Observability
## 12. Rollback
## 13. Security requirements
## 14. Operational procedures
## 15. Current deployment status
## 16. Known deployment risks
## 17. Pending deployment work
## 18. Document boundary
```

Section rules:

- Never expose secret values.
- Separate local, development, staging and production behavior.
- Do not claim a deployment occurred without explicit evidence.

---

## 10. Project and suite `README.md`

Required structure:

```text
# Project or suite name

## Overview
## Purpose
## Architecture
## Requirements
## Configuration
## Installation
## Runtime
## Usage
## API or interfaces
## Development
## Validation
## Security
## Known limitations
## Related documentation
```

Section rules:

- README files describe stable user-facing behavior.
- Do not include temporary implementation notes.
- Do not include historical chat decisions.
- Omit sections that are genuinely not applicable only when the source README already omits them.

---

## 11. `FORMAT_CONTEXT.md`

Required structure:

```text
# FORMAT_CONTEXT.md

## 1. Global rules
## 2. Global PROJECT_CONTEXT.md
## 3. Global SUITE_CONTEXT.md
## 4. Global BUSINESS_CONTEXT.md
## 5. Global QA_CONTEXT.md
## 6. Global SYS_PROMPT.md
## 7. Project context/PROJECT_CONTEXT.md
## 8. Project context/QA_CONTEXT.md
## 9. Project context/DEPLOY_CONTEXT.md
## 10. Project and suite README.md
## 11. FORMAT_CONTEXT.md
## 12. Enforcement rules
## 13. Document boundary
```

---

## 12. Enforcement rules

Every context export and upgrade workflow must:

1. Include this file as a protected format contract.
2. Make it available to RAG retrieval.
3. Include its complete contents in the export package.
4. Never allow ChatGPT to modify it through `context-upgrade`.
5. Validate every generated context against its required heading structure.
6. Reject files with missing, renamed, duplicated or reordered required headings.
7. Allow content changes only inside existing sections.
8. Reject unexpected top-level sections unless this contract explicitly allows them.
9. Report structural validation errors before replacement.
10. Keep the input ZIP untouched when validation fails.
11. Apply replacements only after all files pass structural validation.
12. Preserve backup, rollback and atomic replacement behavior.

---

## 13. Document boundary

This file defines structure only.

It does not define:

- business behavior;
- architecture decisions;
- QA results;
- deployment status;
- implementation completion;
- project priorities.
