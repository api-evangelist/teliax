---
generated: '2026-10-09'
method: generated
name: teliax-toll-free-routing-resolution
description: Resolve a toll-free number with Telique RouteLink. Find its Responsible Org and carrier (CIC), dump its CPR decision tree, and list everything that RespOrg manages.
api: openapi/teliax-telique-openapi.yml
operations: [lookupRor, lookupCicRor, dumpCpr, lookupTfnsByRor, lookupCprsByRor]
source: Grounded in openapi/teliax-telique-openapi.yml. Every operationId was checked verbatim in the spec. MCP equivalents are routelink_lookup, routelink_cpr and routelink_ror_query (mcp/teliax-tool-crosswalk.yml).
---

# Toll-free routing resolution

## Auth
- Base URL `https://api.telique.ringer.tel`, with the `x-api-token` header (anonymous calls: 10 requests/minute).

## Steps
1. **Who owns it**: `lookupRor` (`GET /v1/telique/ror/{crn}`). Returns the Responsible Organization for the toll-free number.
2. **Where a call routes**: `lookupCicRor` (`GET /v1/telique/cicror/{crn}/{ani}/{lata}`). Returns the carrier (CIC) and RespOrg for a call from a given ANI and LATA.
3. **Full decision tree**: `dumpCpr` (`GET /v1/telique/cpr/{crn}`). Dumps the Call Processing Record (use `expand` to expand it).
4. **RespOrg portfolio**: `lookupTfnsByRor` (`GET /v1/telique/ror/{ror}/tfns`) and `lookupCprsByRor` (`GET /v1/telique/ror/{ror}/cprs`).

## Notes
- All operations here are read-only GETs and safe to retry.
