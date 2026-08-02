# BUSINESS_CONTEXT.md

> **Last updated:** 2026-07-30
>
> **Purpose**
>
> Persistent business context for **SBM Suite**. It defines brands, franchises, operational profiles, enabled modules, business domains, entities, rules and commercial boundaries.
>
> **Accuracy note**
>
> Business counts, module status and operational metrics must come from validated evidence or authoritative systems. Unknown values remain `N/A`.

## 1. Business overview

SBM Suite is a configurable ERP platform designed to support multiple businesses, brands, franchises and operational models from a shared technological ecosystem.

Core separation:

```text
Client business operation
→ client-facing application and API

Platform or contractual operation
→ internal SBM administration
```

The platform must reduce manual dependence on SBM personnel for routine client operations without exposing critical platform controls.

## 2. Product vision

Provide a multi-brand platform where each authorized business can operate independently while SBM retains control over:

- tenant and franchise provisioning;
- subscriptions and plans;
- contracted modules;
- global configuration;
- internal administration;
- shared platform services;
- platform-level audit and support.

## 3. Business actors

| Actor | Scope | Main responsibilities | Restrictions |
|---|---|---|---|
| Client user | Own brand or tenant | Operate products, materials, services, catalogs, prices, providers, branches, tickets and approved AI workflows | No cross-brand or platform administration |
| Client administrator | Own brand or tenant | Manage permitted users, roles, modules and operational configuration | Cannot provision tenants, franchises or uncontracted modules |
| Internal SBM user | Platform | Provision clients, franchises, subscriptions, modules and global services | Must remain within internal authority |
| AI-assisted user | Inherited user scope | Perform approved operations through Tools and responsible APIs | AI gains no independent authority |

Authorization target:

```text
requesting user
→ tenant or franchise
→ active modules
→ role
→ permission
→ restriction
→ approved business action
```

## 4. Brands and franchises

| Brand ID | Brand | Franchise | Description | Status | Source |
|---|---|---:|---|---|---|
| SBM | SBM | 0 | Platform owner, internal services and shared infrastructure | active | Suite context |
| DITALY-PASTA | Ditaly Pasta | 1 | Initial validated food-service business operating on SBM Suite | active | Current business context |

Rules:

- `Franchise` uses `1 = true`, `0 = false`.
- Every brand operates within isolated business and authorization boundaries.
- A shared physical schema does not authorize cross-brand access.
- New brands or franchise changes require updating this context and related documentation.

## 5. Brand operational profile

| Brand | Locales enabled | Local count | Client count | Product count | Ticket count | Stock tracked | Last updated | Source |
|---|---:|---:|---:|---:|---:|---:|---|---|
| SBM | 0 | N/A | N/A | N/A | N/A | 0 | 2026-07-30 | Context definition |
| Ditaly Pasta | 1 | N/A | N/A | N/A | N/A | N/A | 2026-07-30 | Authoritative endpoint pending |

Rules:

- Boolean values use `1 = true`, `0 = false`.
- Unknown counts use `N/A`.
- Future endpoint-driven values must include source and update timestamp.
- Counts must never be inferred from code, filenames or incomplete database evidence.

## 6. Enabled modules by brand

| Brand | Module | Enabled | Description | Effective date | Source |
|---|---|---:|---|---|---|
| Ditaly Pasta | Product | 1 | Sellable item management | N/A | Current validated domain |
| Ditaly Pasta | Material | 1 | Ingredient and operational input management | N/A | Current validated domain |
| Ditaly Pasta | Service | 1 | Non-physical business offering management | N/A | Current validated direction |
| Ditaly Pasta | Catalog | 1 | Grouping and publication of offerings | N/A | Current validated direction |
| Ditaly Pasta | Ticket | 1 | Client-facing operational and support requests | N/A | Current validated direction |
| Ditaly Pasta | Pricing | 1 | Price, tax and fiscal configuration | N/A | Current validated domain |
| Ditaly Pasta | Provider | 1 | Provider and related business information | N/A | Current validated domain |
| Ditaly Pasta | Branch | 1 | Physical or operational locations | N/A | Current validated domain |
| Ditaly Pasta | Agreement | 1 | Commercial relationship configuration | N/A | Current validated domain |

Rules:

- A module change updates this context when business capability changes.
- A technical implementation change alone does not change module status.
- Module activation remains platform-controlled when contractually applicable.

## 7. Core business domains

Canonical client-facing domains:

```text
Product
Material
Service
Catalog
Ticket
Price
Provider
Branch
Agreement
User and authorization
```

Canonical ownership currently defined:

```text
Product  → products app
Material → material app
Service  → service app
Catalog  → catalog app
Ticket   → ticket app
```

Each domain remains independent even when models share common fields.

## 8. Business entities

| Entity | Description | Client-facing owner | Platform/internal owner | Main lifecycle |
|---|---|---|---|---|
| Product | Sellable business item | DP-API | Internal support only when explicitly required | create, confirm, update, version, soft-delete |
| Material | Input used in production, packaging or operations | DP-API | Internal support only when explicitly required | create, confirm, update, version, soft-delete |
| Service | Non-physical commercial or operational offering | DP-API | Internal support only when explicitly required | create, confirm, update, version, soft-delete |
| Catalog | Grouping and publication of offerings | DP-API | Global policy only when explicitly platform-owned | create, configure, publish, deactivate |
| Ticket | Client-facing operational or support request | DP-API | Internal escalation or support workflow | create, assign, progress, resolve |
| Price | Monetary state of a priced business record | DP-API | Global fiscal policy where applicable | create version, activate current, preserve history |
| Provider | Supplier of products, materials or services | DP-API | Shared references where applicable | create, update, deactivate |
| Branch | Physical or operational business location | DP-API | Tenant provisioning remains internal | create, configure, activate, deactivate |
| Agreement | Commercial relationship and applicable conditions | DP-API | Contractual policy where applicable | create, activate, expire |
| Franchise | Contractual business unit | Not client-controlled | SBM-API | provision, configure, activate, deactivate |
| Tenant | Isolated operational business scope | Not client-controlled | SBM-API | provision, configure, activate, deactivate |

## 9. Business rules

1. Client users operate only within their tenant or brand.
2. Routine client operations should not require internal SBM intervention.
3. Product, Material, Service, Catalog and Ticket remain separate capabilities.
4. Similar fields do not justify merging domains.
5. Each business capability has one canonical owner.
6. Price calculations and versioning remain backend responsibilities.
7. Audit and confirmation metadata are server-controlled.
8. AI actions use the same permissions as direct user actions.
9. Frontends do not reproduce authoritative backend rules.
10. Platform provisioning remains internal.
11. Physical schema location does not determine business ownership.
12. Legacy data must not be silently deleted or hidden.
13. Business changes must remain traceable.
14. Cross-brand access is prohibited unless explicitly designed.
15. Business capability changes must update this context and related documentation.

## 10. Commercial flows

Client operation:

```text
Client user
→ SBM Manager or approved channel
→ DP-API
→ validated business operation
→ persisted business state
```

Internal platform operation:

```text
Internal SBM user
→ SBM Manager or internal channel
→ SBM-API
→ provisioning, subscription or global configuration
```

AI-assisted operation:

```text
Authorized user
→ SBM AI Assistant
→ explicit Tool
→ responsible API
→ validated result
```

The AI must not invent business identifiers, bypass validation or exceed the requesting user's authority.

## 11. Pricing and fiscal concepts

A Price may include:

- base net amount;
- net amount;
- tax amount;
- additional tax;
- retention;
- gross amount;
- price configuration;
- record type;
- referenced item code;
- current-version state;
- confirmation state;
- audit information.

Rules:

- pricing calculations are authoritative in the backend;
- formula evaluation must be deterministic;
- monetary values use exact decimal handling;
- price history must remain auditable;
- only compatible and confirmed configurations are accepted;
- frontends and AI assistants must not reproduce fiscal logic.

Expected history flow:

```text
current price
→ business value changes
→ create new price version
→ link business record to new price
→ previous owned price becomes non-current
```

## 12. Inventory and catalog concepts

Inventory concepts may include:

- stock;
- availability;
- material consumption;
- package and unit of measure;
- branch-specific availability;
- provider and dispatch data.

Catalog concepts may include:

- products;
- services;
- materials when commercially applicable;
- menus;
- groups;
- categories;
- visibility rules;
- publication state;
- branch or channel conditions.

Rules:

- Catalog does not own Product, Material or Service lifecycle.
- Stock values must come from authoritative operational sources.
- Catalog visibility may depend on branch, channel, franchise, state or configuration.

## 13. Sales and order concepts

Sales and order capabilities may include:

- priced items;
- catalogs;
- branch availability;
- agreements;
- discounts;
- fiscal configuration;
- tickets or support related to an order.

No complete sales or order workflow is considered validated unless explicitly evidenced by the responsible project and database contexts.

## 14. Provider and branch concepts

Provider rules:

- providers are managed by authorized client users;
- provider changes preserve referential integrity;
- providers may be referenced by Products, Materials and Services;
- shared provider data does not merge domain ownership.

Branch rules:

- branch data belongs to the client business;
- branch access remains tenant-scoped;
- catalogs, prices, channels and integrations may vary by branch;
- platform provisioning remains internal.

## 15. Documentation references

Relevant documentation must use repository-relative paths under:

```text
SBM-SUITE/context/documentation/
```

Business-related documentation domains include:

- SBM Suite;
- Roadmap;
- Development;
- Technologies;
- Business modules;
- Brands and franchises;
- Security and DevSecOps;
- QA and Testing.

Specific page and subpage paths must be added when the documentation tree format is finalized.

## 16. Terminology

| Term | Meaning |
|---|---|
| Brand | Business identity operating on SBM Suite |
| Franchise | Contractual business unit provisioned by SBM |
| Tenant | Isolated operational scope for a client |
| Module | Enabled business capability |
| Client user | User operating within one authorized business scope |
| Internal SBM user | User managing platform-level operations |
| Product | Sellable business item |
| Material | Input used in production or operations |
| Service | Non-physical business offering |
| Catalog | Published grouping of offerings |
| Ticket | Operational or support request |
| Price | Monetary state and history of a priced record |
| Branch | Physical or operational location |
| Agreement | Commercial relationship and applicable conditions |

## 17. Validated business decisions

| Decision | Status | Business effect | Source |
|---|---|---|---|
| Client operations belong to DP-API | accepted | Routine business operations remain client-facing | Current architecture |
| Platform provisioning belongs to SBM-API | accepted | Tenants, franchises and contracted modules remain internal | Current architecture |
| Product, Material, Service, Catalog and Ticket remain separate domains | accepted | Independent lifecycle and ownership | Current business direction |
| Ditaly Pasta is the initial validated business | accepted | First configured brand and operational model | Existing context |
| Git is the current source of truth for business context and documentation | accepted | Changes are versioned before future API synchronization | Current workflow |

## 18. Business constraints

- Business metrics are not yet populated from an authoritative endpoint.
- Multi-brand isolation must be enforced explicitly.
- Legacy data may contain inconsistencies.
- Service fields and relationships still require database validation.
- Contracted-module state remains platform-controlled.
- Documentation synchronization is manual in the first stage.
- Unknown operational counts remain `N/A`.

## 19. Pending business definitions

- authoritative endpoint for brand operational metrics;
- exact local, client, product, ticket and stock counts;
- definitive Service fields and relationships;
- complete sales and order workflows;
- formal module activation rules per brand;
- future brands and franchise profiles;
- automated Git-to-Notion synchronization;
- bidirectional conflict management between Git and Notion.

## 20. Document boundary

This file stores business meaning, brands, franchises, capabilities, entities, rules and operational profiles.

It does not define:

- technical architecture;
- endpoint implementation details;
- container topology;
- source code structure;
- QA execution results;
- deployment procedures;
- security control implementation;
- data schema ownership;
- documentation page content.

Those concerns belong to their corresponding Suite, Project, QA, Security, Data, Deploy and Documentation contexts.
