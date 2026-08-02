# QA_CONTEXT.md

> **Last updated:** 2026-07-30
>
> **Purpose**
>
> Transversal QA context for **SBM Suite**. It summarizes quality policy, project QA status, test inventory, coverage, SonarQube, security validation, API validation, database validation, release criteria and accepted risks.
>
> **Accuracy note**
>
> Only explicitly executed and evidenced results may be recorded as validated. Unknown counts, coverage, SonarQube results and dates remain `N/A`.

## 1. Suite QA overview

Global QA applies to:

- frontend-to-API validation;
- API-to-database integration;
- cross-API workflows;
- AI Tool-to-API validation;
- authentication and authorization;
- tenant and brand isolation;
- shared Docker services;
- transversal smoke tests;
- multi-repository regression;
- release acceptance involving more than one project.

Project-specific test plans, fixtures, commands and detailed evidence remain in each project's `context/QA_CONTEXT.md`.

## 2. Quality policy

1. Validate behavior through public contracts.
2. Test the canonical owner.
3. Preserve project and business ownership boundaries.
4. Use real integration boundaries when practical.
5. Keep test data isolated and deterministic.
6. Never mutate production or shared persistent data during QA.
7. Never execute unauthorized migrations from application repositories.
8. Validate success and failure paths.
9. Validate permissions and tenant isolation.
10. Record exact commands, evidence and results.
11. Do not treat passing unit tests as proof of transversal compatibility.
12. Keep project and global QA contexts synchronized.
13. Do not hide code from coverage to improve metrics.
14. Do not mark planned work as executed.
15. Work one validated step at a time.

## 3. Quality gates

| Gate | Scope | Required evidence | Blocking |
|---|---|---|---:|
| Test execution | Project and transversal | Command, passed, failed, skipped and timestamp | 1 |
| Coverage | Project | Coverage report and threshold result | 1 |
| Static analysis | Project | Tool output and issue count | 1 |
| SonarQube | Project | Quality Gate status and project key | 1 |
| API contract | Affected API | Request, response, status and error validation | 1 |
| Database compatibility | Database-sensitive changes | Schema, Flyway, DBML and model comparison | 1 |
| Security | Protected flows | Authentication, authorization and isolation validation | 1 |
| Integration | Cross-project flow | Consumer-to-provider evidence | 1 |
| Failure paths | Changed behavior | Negative scenarios and rollback behavior | 1 |
| Documentation | Context and documentation lifecycle | Updated authorized artifacts | 1 |

A gate may be bypassed only through a documented accepted exception.

## 4. Project QA summaries

| Project | QA context | Test count | Passed | Failed | Coverage | SonarQube status | Last execution | Overall risk | Evidence |
|---|---|---:|---:|---:|---|---|---|---:|---|
| DP-API | `SBM-SUITE/DP-API/context/QA_CONTEXT.md` | N/A | N/A | N/A | N/A | N/A | N/A | 3 | QA procedure objective pending |
| SBM-API | `SBM-SUITE/SBM-API/context/QA_CONTEXT.md` | N/A | N/A | N/A | N/A | N/A | N/A | 3 | Project QA context pending |
| SBM-MANAGER | `SBM-SUITE/SBM-MANAGER/context/QA_CONTEXT.md` | N/A | N/A | N/A | N/A | N/A | N/A | 3 | Project QA context pending |
| SBM-DB | `SBM-SUITE/SBM-DB/context/QA_CONTEXT.md` | N/A | N/A | N/A | N/A | N/A | N/A | 4 | Database QA context pending |
| SBM-AI-ASSISTANT | `SBM-SUITE/sbm-ai-assistant/context/QA_CONTEXT.md` | N/A | N/A | N/A | N/A | N/A | N/A | 3 | Project QA context pending |

Risk scale:

```text
0 = none
1 = very low
2 = low
3 = medium
4 = high
5 = critical
```

## 5. Test inventory

| Test ID | Project | Description | Logic type | Components | Risk | Last execution | Result | Evidence |
|---|---|---|---|---|---:|---|---|---|
| QA-CONTEXT-001 | SBM Suite | Validate project QA context synchronization with global QA context | integration | context-deploy, context-upgrade, QA contexts | 4 | N/A | pending | No execution evidence |
| QA-CONTEXT-002 | SBM Suite | Validate project context synchronization with global project context | integration | project contexts, context-upgrade | 4 | N/A | pending | No execution evidence |
| QA-CONTEXT-003 | SBM Suite | Validate section patch structure and authorized paths | security | manifest, ZIP, patch validator | 5 | N/A | pending | No execution evidence |
| QA-CONTEXT-004 | SBM Suite | Validate context backup and atomic rollback | integration | context-upgrade, filesystem | 5 | N/A | pending | No execution evidence |
| QA-DOC-001 | SBM Suite | Validate documentation package authorized Markdown paths | security | documentation-upgrade, manifest | 5 | N/A | pending | Workflow not implemented |
| QA-DOC-002 | SBM Suite | Validate documentation backup and replacement | integration | documentation-upgrade, filesystem | 4 | N/A | pending | Workflow not implemented |
| QA-API-001 | SBM Suite | Validate frontend-to-canonical API routing | api | SBM-MANAGER, DP-API, SBM-API | 4 | N/A | pending | No transversal evidence |
| QA-AI-001 | SBM Suite | Validate AI Tool uses canonical API without direct database access | security | SBM-AI-ASSISTANT, Tools, APIs | 5 | N/A | pending | Tool integration pending |
| QA-TENANT-001 | SBM Suite | Deny cross-tenant read and write operations | security | authentication, authorization, APIs | 5 | N/A | pending | No transversal evidence |
| QA-DB-001 | SBM Suite | Validate application models against PostgreSQL, Flyway and DBML | database | DP-API, SBM-API, SBM-DB | 5 | N/A | pending | No transversal evidence |

Allowed logic types:

```text
unit
integration
api
database
security
static-analysis
coverage
deployment
```

## 6. Coverage summary

| Project | Tool | Coverage | Threshold | Status | Last execution | Evidence |
|---|---|---|---|---|---|---|
| DP-API | N/A | N/A | N/A | not validated | N/A | No coverage evidence supplied |
| SBM-API | N/A | N/A | N/A | not validated | N/A | No coverage evidence supplied |
| SBM-MANAGER | N/A | N/A | N/A | not validated | N/A | No coverage evidence supplied |
| SBM-DB | N/A | N/A | N/A | not validated | N/A | No coverage evidence supplied |
| SBM-AI-ASSISTANT | N/A | N/A | N/A | not validated | N/A | No coverage evidence supplied |

Coverage rules:

- `qa-check.sh` generates coverage evidence.
- Coverage must use the real project configuration.
- Exclusions require documented justification.
- Coverage percentage alone does not prove contract or integration quality.

## 7. Static analysis summary

| Project | Tool | Project key | Status | Issues | Last execution | Evidence |
|---|---|---|---|---:|---|---|
| DP-API | SonarQube | N/A | not validated | N/A | N/A | No SonarQube evidence supplied |
| SBM-API | SonarQube | N/A | not validated | N/A | N/A | No SonarQube evidence supplied |
| SBM-MANAGER | SonarQube | N/A | not validated | N/A | N/A | No SonarQube evidence supplied |
| SBM-DB | N/A | N/A | not validated | N/A | N/A | No static-analysis evidence supplied |
| SBM-AI-ASSISTANT | SonarQube | N/A | not validated | N/A | N/A | No SonarQube evidence supplied |

Rules:

- Quality Gate status must come from actual scanner output.
- Scanner failure is not equivalent to a failed Quality Gate.
- Project keys, URLs and credentials must not be invented or exposed.

## 8. Security validation summary

Required transversal checks:

- unauthenticated requests;
- invalid or expired credentials;
- authenticated but unauthorized requests;
- wrong tenant or brand;
- missing module;
- insufficient role;
- object-level restrictions;
- client versus internal permissions;
- AI-triggered actions using identical authorization rules;
- ZIP path traversal;
- symlinks;
- absolute paths;
- unauthorized patch targets;
- secret leakage.

Current status:

```text
No suite-wide security validation evidence supplied.
```

## 9. API validation summary

Required checks:

- canonical API owner;
- HTTP method and path;
- request body;
- response body;
- status codes;
- public error contract;
- authentication;
- authorization;
- pagination and filtering;
- idempotency;
- compatibility with existing consumers.

Any method, path, request body or response contract change must update `SUITE_CONTEXT.md` and corresponding QA records.

Current status:

```text
No complete transversal API validation evidence supplied.
```

## 10. Database validation summary

Mandatory comparison:

```text
current PostgreSQL schema
↔ current Flyway scripts
↔ current DBML
↔ application model
↔ serializer and public contract
```

Required checks:

- field names and types;
- nullability;
- foreign keys;
- constraints and indexes;
- generated identifiers;
- triggers;
- transactions;
- logical deletion;
- audit and version fields;
- unmanaged model configuration;
- absence of unauthorized migrations.

Current status:

```text
No complete transversal database validation evidence supplied.
```

## 11. Deployment validation summary

Required checks:

- required containers are running;
- container names and ports do not conflict;
- internal services resolve through Docker;
- dependencies become ready;
- health checks respond;
- environment variables load from the expected source;
- failure of one dependency produces a controlled error;
- rollback and backup behavior work;
- no secret values appear in logs or generated packages.

Known shared network:

```text
sbm-network
```

Current status:

```text
No complete transversal deployment validation evidence supplied.
```

## 12. Defect classification

| Severity | Name | Definition | Release effect |
|---:|---|---|---|
| 0 | none | No observed defect | none |
| 1 | very low | Cosmetic or negligible operational effect | normally non-blocking |
| 2 | low | Limited non-critical behavior affected | may be accepted |
| 3 | medium | Relevant feature degradation with workaround | requires explicit decision |
| 4 | high | Major capability, security or integration failure | blocking |
| 5 | critical | Data loss, privilege bypass, cross-tenant access or system-wide failure | blocking |

Every defect must include:

- affected project;
- description;
- reproduction evidence;
- severity;
- owner;
- status;
- accepted workaround if applicable.

## 13. Risk classification

| Risk | Meaning | Expected action |
|---:|---|---|
| 0 | none | no action |
| 1 | very low | monitor |
| 2 | low | plan correction |
| 3 | medium | explicit owner and mitigation |
| 4 | high | block unless accepted |
| 5 | critical | mandatory block |

Risk values must be integers from `0` to `5`.

## 14. Release criteria

Release statuses:

```text
PASS
→ all required criteria validated

PASS WITH ACCEPTED RISK
→ required criteria validated except documented non-blocking risks

BLOCKED
→ dependency, environment or source unavailable

FAIL
→ observed behavior violates the accepted contract
```

A release requires:

- applicable project quality gates;
- transversal integration checks when relevant;
- security validation;
- failure-path validation;
- database impact statement;
- migration statement;
- coverage and static-analysis evidence when configured;
- updated contexts;
- updated documentation when required;
- no unaccepted high or critical risk.

## 15. Accepted exceptions

| Exception ID | Scope | Description | Risk | Reason | Owner | Expiration | Status |
|---|---|---|---:|---|---|---|---|
| N/A | N/A | No accepted QA exceptions currently evidenced | 0 | N/A | N/A | N/A | none |

An exception must never be inferred from missing evidence.

## 16. Current QA status

Current suite QA state:

```text
Status: BLOCKED
Reason: complete project-level test, coverage and SonarQube evidence is not yet supplied.
```

Verified workflow responsibility:

```text
qa-check.sh
→ execute tests
→ generate coverage
→ execute SonarQube scanner
→ produce QA evidence

context-deploy.sh
→ extract and package QA evidence

context-upgrade.sh
→ update project and global QA contexts
```

No test, coverage or SonarQube result is marked as completed by this context update.

## 17. Pending QA work

1. Define the complete DP-API QA procedure.
2. Standardize `qa-check.sh` evidence output.
3. Define project-specific coverage thresholds.
4. Define SonarQube project keys and mandatory gates.
5. Build project test inventories.
6. Implement global and project QA context synchronization.
7. Validate section patch import security.
8. Validate context backup and rollback.
9. Implement documentation upgrade QA.
10. Add transversal tenant-isolation tests.
11. Add frontend-to-API contract tests.
12. Add AI Tool-to-API authorization tests.
13. Add API-to-database compatibility tests.
14. Create QA contexts for remaining projects.

## 18. Related documentation

Relevant documentation domains include:

- QA and Testing;
- Security and DevSecOps;
- Development;
- Roadmap;
- Observability;
- DevOps;
- SBM Suite.

Paths must use:

```text
SBM-SUITE/context/documentation/<page>/<page>.md
SBM-SUITE/context/documentation/<page>/subpages/<subpage>.md
```

Specific paths will be added when the documentation format and tree are finalized.

## 19. Document boundary

This file stores transversal QA policy, summarized project status, test inventory, quality gates, evidence state, risks and release criteria.

It does not replace:

- detailed project test plans;
- project fixtures;
- project commands;
- raw coverage reports;
- SonarQube reports;
- deployment instructions;
- security architecture;
- database schema definitions;
- documentation page content.

Detailed evidence remains in each project QA context and generated QA artifacts.
