# QA_CONTEXT.md

> **Last updated:** 2026-07-29
>
> **Purpose**
>
> This document defines the transversal QA rules for **SBM Suite**. It applies when a feature, workflow, contract, deployment, or business operation involves one or more repositories.
>
> Project-specific tests and quality rules belong to each repository's own `context/QA_CONTEXT.md`. This global context coordinates shared acceptance criteria and cross-project validation.

---

## 1. QA scope

Use this context for:

- frontend-to-API validation;
- API-to-database integration;
- cross-API workflows;
- AI Tool-to-API validation;
- authentication and authorization across components;
- tenant and brand isolation;
- shared Docker network and service communication;
- transversal smoke tests;
- multi-repository regression testing;
- release acceptance involving more than one project.

Do not use this file as the only QA source for a repository. Always combine it with the local project context.

---

## 2. Context reading order

Before planning transversal QA, read:

```text
SBM-SUITE/project_context.md
SBM-SUITE/context/SUITE_CONTEXT.md
SBM-SUITE/context/BUSINESS_CONTEXT.md
SBM-SUITE/context/QA_CONTEXT.md
<project>/context/PROJECT_CONTEXT.md
<project>/context/QA_CONTEXT.md
<project>/context/DEPLOY_CONTEXT.md
```

For a workflow involving multiple projects, read the local context of every affected repository.

If two contexts conflict:

1. identify the conflict;
2. verify the current code, runtime, contracts and database;
3. do not assume which source is correct;
4. update the stale context after validation.

---

## 3. QA ownership model

```text
Global transversal QA
→ SBM-SUITE/context/QA_CONTEXT.md

Repository-specific QA
→ <project>/context/QA_CONTEXT.md

Deployment validation
→ <project>/context/DEPLOY_CONTEXT.md

Business acceptance
→ SBM-SUITE/context/BUSINESS_CONTEXT.md
```

The global context defines cross-project expectations.

The local context defines:

- exact commands;
- test suites;
- coverage targets;
- SonarQube configuration;
- fixtures;
- repository-specific risks;
- local acceptance criteria.

---

## 4. Core transversal principles

1. Validate behavior through public contracts.
2. Do not test a component in isolation when the change crosses repositories.
3. Preserve canonical ownership.
4. Do not bypass the responsible API.
5. Use the real integration boundary whenever practical.
6. Keep test data isolated and deterministic.
7. Do not mutate production or shared persistent data during QA.
8. Do not run database migrations from application repositories when Flyway owns the schema.
9. Verify both success and failure paths.
10. Validate permissions and tenant isolation.
11. Record exact commands and results.
12. Stop when a required dependency or source of truth is missing.
13. Do not treat a passing local unit test as proof of cross-suite compatibility.
14. Do not modify unrelated projects during QA.
15. Update affected QA contexts after acceptance.

---

## 5. Standard transversal flow

For any multi-project change:

```text
identify affected repositories
→ identify canonical owner
→ identify public contracts
→ verify current database source
→ define test matrix
→ validate each repository locally
→ validate integrations
→ validate failure paths
→ validate permissions and isolation
→ validate observability
→ document results
```

---

## 6. Required QA matrix

Every transversal QA plan must include:

| Area | Validation |
|---|---|
| Ownership | Correct project and domain own the behavior |
| Contract | Request, response, status and error format |
| Integration | Real consumer reaches the responsible provider |
| Database | Mapping matches current PostgreSQL and Flyway |
| Security | Authentication, authorization and tenant isolation |
| Failure | Invalid input and unavailable dependency behavior |
| Audit | User, timestamp and action are traceable |
| Idempotency | Repeated requests do not create unintended effects |
| Compatibility | Existing consumers continue working |
| Observability | Logs and errors identify the failing component |
| Documentation | Contexts and README remain synchronized |

---

## 7. Frontend-to-API QA

Applicable flow:

```text
SBM-MANAGER
→ DP-API or SBM-API
```

Required checks:

- frontend calls the canonical API;
- endpoint path and HTTP method are correct;
- request payload matches the backend contract;
- response fields are correctly interpreted;
- validation errors are displayed;
- loading and empty states work;
- permissions are respected;
- disabled operations are not exposed;
- frontend does not calculate backend-owned values;
- frontend does not fabricate identifiers or audit fields;
- logical deletion behavior is reflected correctly;
- stale endpoints are removed only after all consumers migrate.

---

## 8. API-to-database QA

Applicable flow:

```text
DP-API / SBM-API
→ PostgreSQL
```

Required checks:

- Django or application mapping matches the live table;
- field names, types, nullability and foreign keys are correct;
- unmanaged models remain unmanaged when Flyway owns the table;
- triggers and generated identifiers behave as expected;
- transactions preserve consistency;
- logical deletion does not physically remove records;
- versioning and audit fields update correctly;
- legacy data remains readable when compatibility is required;
- tests do not alter shared persistent data;
- no unauthorized migration is generated or applied.

Mandatory comparison for database-sensitive work:

```text
current PostgreSQL schema
↔ current Flyway scripts
↔ current DBML
↔ application model
↔ serializer and public contract
```

---

## 9. Cross-API QA

Applicable flow:

```text
DP-API
↔ SBM-API
```

Use only when a workflow explicitly requires both APIs.

Required checks:

- ownership remains unambiguous;
- no duplicated write occurs;
- one API does not bypass the other's validations;
- authentication context is propagated safely;
- failures are not silently converted to success;
- retries do not create duplicate operations;
- synchronous dependencies have documented timeout and error behavior;
- circular dependencies are avoided;
- audit records identify the initiating user and component.

---

## 10. AI Tool QA

Applicable flow:

```text
SBM-AI-ASSISTANT
→ Tool
→ DP-API or SBM-API
```

Required checks:

- intent routes to the correct Tool;
- Tool calls the canonical API;
- structured input matches the API contract;
- identifiers are resolved, not invented;
- requesting-user identity and tenant context are preserved;
- permissions match direct API usage;
- sensitive writes require approval when defined;
- API validation errors are returned faithfully;
- rejected operations remain rejected;
- AI does not access PostgreSQL directly;
- AI does not reproduce authoritative business calculations;
- tool name, parameters, result, user and timestamp are observable;
- repeated requests are idempotent where required.

---

## 11. Tenant and brand isolation QA

Every client-facing transversal test must verify isolation.

Required negative case:

```text
User from Tenant A
→ attempts to read or modify Tenant B
→ access denied or resource hidden
```

Validate isolation in:

- list endpoints;
- detail endpoints;
- search;
- filters;
- exports;
- admin;
- background tasks;
- AI Tools;
- logs and audit records;
- cached or vectorized data when tenant-specific.

Shared lookup data must be explicitly identified as global.

---

## 12. Authentication and authorization QA

Required flow:

```text
identity
→ tenant/franchise
→ active modules
→ role
→ permission
→ restriction
→ requested object
```

Validate:

- unauthenticated requests;
- authenticated but unauthorized requests;
- expired or invalid credentials;
- wrong tenant;
- missing module;
- insufficient role;
- object-level restrictions;
- admin versus client permissions;
- AI-triggered actions using the same authorization rules.

Do not use unrestricted technical credentials as proof that a client workflow works.

---

## 13. Business lifecycle QA

For entities with lifecycle behavior, validate:

```text
create
→ read
→ partial update
→ confirmation
→ version increment
→ logical deletion
```

Required checks:

- server-generated identifiers;
- immutable fields;
- confirmation metadata;
- audit log;
- effective versus idempotent changes;
- logical deletion visibility;
- disabled physical deletion;
- transaction rollback;
- concurrent update protection where applicable.

---

## 14. Pricing QA

For Product, Material, Service, or other priced entities:

- validate compatible Price Configuration;
- validate exact decimal handling;
- validate derived amounts;
- validate Price version creation;
- validate previous current Price transition;
- validate shared or legacy Price protection;
- validate idempotent price updates;
- validate transaction rollback;
- validate fiscal configuration ownership;
- validate frontend and AI do not calculate authoritative values independently.

---

## 15. Contract compatibility QA

Before retiring or moving an endpoint:

1. identify every consumer;
2. compare old and new contracts;
3. validate request compatibility;
4. validate response compatibility;
5. validate errors and status codes;
6. migrate consumers;
7. run regression tests;
8. monitor the replacement;
9. retire the duplicate only after acceptance.

Never remove a duplicate implementation merely because the new endpoint exists.

---

## 16. Docker and service QA

Validate the real local topology when the task affects runtime communication.

Known shared network:

```text
sbm-network
```

Check:

- required containers are running;
- container names are unique;
- host ports do not conflict;
- internal ports are reachable;
- DNS names resolve through Docker;
- environment variables are loaded from the expected file;
- dependent services are ready;
- health endpoints respond;
- logs show no hidden startup failures;
- stopping one dependency produces a controlled error.

Do not assume current ports or names without checking the corresponding Compose files.

---

## 17. Database test-data rules

Allowed:

- dedicated test database;
- transactional rollback;
- deterministic fixtures;
- isolated seed data;
- read-only inspection of shared development data.

Not allowed:

- destructive tests against production;
- persistent mutation of shared development records;
- manual cleanup as the normal test strategy;
- tests depending on unstable existing rows;
- schema changes from application QA;
- secrets embedded in fixtures.

---

## 18. Failure-path QA

Every transversal workflow must include failure cases.

Examples:

- API unavailable;
- database unavailable;
- invalid payload;
- foreign key missing;
- unauthorized user;
- wrong tenant;
- stale version;
- duplicate request;
- timeout;
- partial dependency failure;
- AI Tool returns malformed response;
- frontend receives HTTP 400, 401, 403, 404, 409 or 500.

Expected behavior:

- no silent success;
- no partial inconsistent write;
- clear error ownership;
- safe rollback;
- useful logs;
- stable public error contract.

---

## 19. Observability QA

For transversal workflows, validate that the failing component can be identified.

Required information where applicable:

- request or correlation ID;
- user;
- tenant;
- component;
- endpoint or Tool;
- action;
- timestamp;
- status;
- error category;
- non-sensitive diagnostic message.

Logs must not expose:

- passwords;
- tokens;
- secrets;
- unrestricted personal data;
- database credentials.

---

## 20. QA execution stages

### 20.1 Development stage

Allowed:

- focused unit tests;
- focused domain tests;
- targeted integration tests;
- system checks;
- import validation;
- endpoint smoke tests.

Follow local project restrictions.

A project context may explicitly forbid:

```text
./scripts/qa-check.sh
```

during development.

### 20.2 Repository acceptance stage

Run the local project's complete QA workflow only after implementation is ready.

Typical checks:

- complete test suite;
- coverage;
- static analysis;
- Django or framework system check;
- local Quality Gate;
- project-specific smoke tests.

### 20.3 Transversal acceptance stage

After local acceptance:

- start required repositories;
- validate the real integration flow;
- execute cross-project smoke tests;
- validate permissions and tenant isolation;
- validate rollback and failure behavior;
- update global and local contexts.

---

## 21. Evidence required

Every completed QA report must include:

```text
scope
affected repositories
tested versions or commit references
environment
commands executed
tests passed and failed
coverage where applicable
static-analysis result
integration scenarios
failure scenarios
database mutation statement
migration statement
known risks
final acceptance status
```

Do not report a test as executed when it was only planned.

---

## 22. Acceptance statuses

Use:

```text
PASS
→ all required criteria validated

PASS WITH ACCEPTED RISK
→ criteria validated except documented non-blocking risk

BLOCKED
→ required dependency, environment or source unavailable

FAIL
→ observed behavior violates the accepted contract
```

Every accepted risk must include:

- description;
- impact;
- reason for acceptance;
- owner;
- future action.

---

## 23. Context update requirements

After transversal QA:

Update the global context when:

- ownership changed;
- a cross-project contract changed;
- a new integration was validated;
- a shared QA standard changed;
- a release risk was accepted;
- a dependency or runtime topology changed.

Update each local QA context when:

- commands changed;
- tests changed;
- coverage changed;
- SonarQube changed;
- project-specific acceptance changed.

Update deployment context when:

- environment variables changed;
- service startup changed;
- ports or containers changed;
- release or rollback procedure changed.

---

## 24. Stable QA rules

1. Test the canonical owner.
2. Test public contracts, not accidental implementation details.
3. Validate integrations after local tests.
4. Preserve tenant isolation.
5. Preserve business auditability.
6. Never bypass APIs for convenience.
7. Never mutate the production schema from application QA.
8. Never hide application code from coverage merely to improve metrics.
9. Never accept silent partial failure.
10. Never remove a legacy endpoint before consumer migration.
11. Record exact evidence.
12. Respect local project QA restrictions.
13. Keep global and local contexts synchronized.
14. Create separate QA contexts for new domains after implementation, not before.
15. Work one validated step at a time.

---

## 25. Current direction

Current and upcoming transversal areas include:

```text
Product
→ DP-API accepted reference
→ SBM-MANAGER consumer validated

Material
→ dedicated DP-API app
→ frontend integration acceptance pending

Service
→ dedicated DP-API app planned
→ database source must be validated first

Context RAG
→ general and project contexts
→ separate Qdrant collection from Confluence business documents
→ ingestion owned by SBM-AI-ASSISTANT
```

Specific QA implementation for Context RAG, Service, Catalog, Ticket, and later domains must be defined in their corresponding project QA contexts after the implementation scope is complete.
