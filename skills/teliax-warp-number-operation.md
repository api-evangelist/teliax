---
generated: '2026-10-09'
method: generated
name: teliax-warp-number-operation
description: Run a bulk number operation on WARP. Check capabilities, preview the change, submit it, then read item results or export them as CSV.
api: openapi/teliax-warp-openapi.yml
operations: [numberOperationsCapabilities, numberOperationsPreview, numberOperationsSubmit, numberOperationsGet, numberOperationsItems, numberOperationsResultsCSV, numberOperationsRetryPreview]
source: Grounded in openapi/teliax-warp-openapi.yml. These are among the only 10 operations in the spec that carry operationIds, and each was checked verbatim. Required scopes are quoted from their descriptions.
---

# WARP number operation

## Auth
- Base URL `https://api.warp.ringer.tel` (overlays/teliax-warp-overlay.yaml makes the scheme-relative server absolute). Send `Authorization: Bearer rk_...`; test keys are `rk_test_...`.
- Read steps need `numbers:read`; preview, submit and retry-preview need `numbers:write`.
- Responses use a `{success, data}` envelope.

## Steps
1. **What is allowed**: `numberOperationsCapabilities` (`GET /v1/number-operations/capabilities`). It still answers when provider configuration disables changes.
2. **Preview**: `numberOperationsPreview` (`POST /v1/number-operations/previews`). Keep the returned operation `id`.
3. **Submit**: `numberOperationsSubmit` (`POST /v1/number-operations/{id}/submit`).
4. **Track**: `numberOperationsGet` (`GET /v1/number-operations/{id}`) and `numberOperationsItems` (`GET /v1/number-operations/{id}/items`).
5. **Results**: `numberOperationsResultsCSV` (`GET /v1/number-operations/{id}/results.csv`).
6. **Failures**: `numberOperationsRetryPreview` (`POST /v1/number-operations/{id}/retry-preview`), then submit again.

## Notes
- The spec says "Operation keys return current persisted state". Re-reading an operation by `id` is safe, so read before you re-submit.
