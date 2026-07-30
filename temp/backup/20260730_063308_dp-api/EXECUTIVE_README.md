# Context Upgrade Summary

**Project:** dp-api  
**Workflow:** context-upgrade  
**Date:** 2026-07-30  
**Affected module:** context deployment script  
**General objective:** Require the canonical format contract during context export.  
**Main completed changes:** `context-deploy.sh` now validates `SYS_PROMPT.md` and `FORMAT_CONTEXT.md`, and sends `format_context_path` to SBM-AI-ASSISTANT.  
**Planned or proposed changes:** None evidenced.  
**Suite-level impact:** Strengthens the shared context export contract.  
**Validated evidence:** QA evidence not supplied.  
**Database or migration impact:** None.  
**Accepted risks:** Runtime behavior was not validated by supplied QA evidence.  
**Main pending work:** Execute focused export and upgrade tests.  
**Updated files:** No context or README replacement included; supplied documents cannot be safely rewritten without broader compliant reconstruction.  
**Proposed commit:** `fix(contexts): require format contract for export`
