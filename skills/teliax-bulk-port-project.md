---
generated: '2026-10-09'
method: generated
name: teliax-bulk-port-project
description: Port a batch of telephone numbers in with TNIQ. Create a bulk port project, load the TNs, validate and fix errors, then submit to SOA and track the lifecycle.
api: openapi/teliax-tniq-openapi.yml
operations: [portWindowCheck, createProject_1, uploadFile, addTns, validateProject, getProjectProgress, getErrorGroups, applyAutoFix, submitProject, getLifecycleSummary, cancelProject]
source: Grounded in openapi/teliax-tniq-openapi.yml (tag Bulk Port). Every operationId was checked verbatim in the spec.
---

# Bulk port project (TNIQ)

## Auth
- Base URL `https://api.tniq.ringer.tel`. Send `Authorization: Bearer <token>`; overlays/teliax-tniq-overlay.yaml applies this scheme API-wide.

## Steps
1. **Preflight the due date**: `portWindowCheck` (`POST /v1/port-projects/port-window-check`).
2. **Create the project**: `createProject_1` (`POST /v1/port-projects/projects`). Keep the `projectId`.
3. **Load numbers**: `uploadFile` (`POST .../{projectId}/upload`, CSV or Excel) or `addTns` (`POST .../{projectId}/tns`, JSON).
4. **Validate**: `validateProject` (`POST .../{projectId}/validate`), then poll `getProjectProgress` (`GET .../{projectId}/progress`).
5. **Fix errors**: `getErrorGroups` (`GET .../{projectId}/error-groups`), then `applyAutoFix` (`POST .../{projectId}/auto-fix`) for errors it can resolve.
6. **Submit**: `submitProject` (`POST .../{projectId}/submit`) sends every valid TN to SOA.
7. **Track**: `getLifecycleSummary` (`GET .../{projectId}/lifecycle-summary`). Abort with `cancelProject` (`POST .../{projectId}/cancel`) if needed.

## Notes
- `submitProject` and `cancelProject` change NPAC/SOA state. Do not blindly retry them; re-read the project status first.
