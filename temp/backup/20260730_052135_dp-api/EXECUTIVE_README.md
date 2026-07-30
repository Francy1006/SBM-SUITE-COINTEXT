# Context Upgrade Summary

- **Project:** dp-api
- **Workflow:** context-upgrade
- **Date:** 2026-07-30
- **Affected module:** context deployment script
- **General objective:** Require the canonical context format contract during context export.
- **Main completed changes:** `scripts/context-deploy.sh` now validates `SYS_PROMPT.md` and `FORMAT_CONTEXT.md`, then sends `/suite/context/FORMAT_CONTEXT.md` through `format_context_path`.
- **Suite-level impact:** The deployment request now participates in the shared context-format validation flow.
- **Validated evidence:** Git diff supplied. QA evidence not supplied.
- **Database or migration impact:** None.
- **Accepted risks:** Runtime endpoint validation was not supplied as QA evidence.
- **Main pending work:** Execute the deployment and upgrade validation suites.
- **Updated files:** No context or README replacement was included. Existing source documents do not match the canonical heading structures and cannot be safely reconstructed from the retrieved evidence without risking information loss.
- **Proposed commit:** `chore(contexts): require format contract during export`
