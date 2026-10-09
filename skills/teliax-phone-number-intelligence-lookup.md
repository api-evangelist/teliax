---
generated: '2026-10-09'
method: generated
name: teliax-phone-number-intelligence-lookup
description: Profile a US phone number with Telique (Ringer, formerly Teliax). Covers LRN and serving carrier, caller name, Do Not Originate status, and the LERG tandem for its NPA-NXX.
api: openapi/teliax-telique-openapi.yml
operations: [lrnLookupByPath, cnamLookup, dnoCheck, queryTandem, identityLookupGet]
source: Grounded in openapi/teliax-telique-openapi.yml. Every operationId was checked verbatim in the spec. MCP equivalents come from mcp/teliax-tool-crosswalk.yml.
---

# Phone number intelligence lookup

## Auth
- Base URL `https://api.telique.ringer.tel`. Send the API token in the `x-api-token` header.
- Without a token, calls are rate-limited to 10 requests per minute. Back off on 429.
- The MCP route is the `lookup_tn` tool, which does all of these lookups in one call. It is served at `https://api.telique.ringer.tel/mcp` (OAuth) or through the `telique-mcp` npm package.

## Steps
1. **Portability / carrier**: `lrnLookupByPath` (`GET /v1/telique/lrn/{phone_number}`). Returns the LRN and serving SPID.
2. **Caller name**: `cnamLookup` (`GET /v1/telique/cnam/{phoneNumber}`). Use `identityLookupGet` (`GET /v1/telique/cnam/identity`) when you also need robocall-mitigation identity data.
3. **Spoofing check**: `dnoCheck` (`GET /v1/telique/dno/{phone_number}`). Treat a DNO hit as a strong spoofing signal for inbound calls.
4. **Routing context**: `queryTandem` (`GET /v1/telique/lerg/tandem?npa=&nxx=`). Returns the tandem switch homing for the number's NPA-NXX.

## Notes
- Each lookup is a read-only GET and safe to retry.
