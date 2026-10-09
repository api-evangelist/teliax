---
generated: '2026-10-09'
method: generated
name: teliax-10dlc-brand-and-campaign
description: Register a 10DLC messaging brand and campaign through TNIQ (TCR). Create and vet the brand, create the campaign, assign numbers, and check carrier (MNO) status.
api: openapi/teliax-tniq-openapi.yml
operations: [createBrand, getBrand, submitVetting, createCampaign, getCampaign, assignNumbers, listAssignedNumberStatuses, getMnoStatus]
source: Grounded in openapi/teliax-tniq-openapi.yml (tags Messaging Brands and Messaging Campaigns). Every operationId was checked verbatim in the spec.
---

# 10DLC brand and campaign (TNIQ)

## Auth
- Base URL `https://api.tniq.ringer.tel`, with `Authorization: Bearer <token>`.

## Steps
1. **Brand**: `createBrand` (`POST /v1/messaging/brands`). Poll `getBrand` (`GET /v1/messaging/brands/{brandId}`) for identity status.
2. **Optional vetting**: `submitVetting` (`POST /v1/messaging/brands/{brandId}/vetting`).
3. **Campaign**: `createCampaign` (`POST /v1/messaging/campaigns`), then `getCampaign` (`GET /v1/messaging/campaigns/{campaignId}`).
4. **Numbers**: `assignNumbers` (`POST /v1/messaging/campaigns/{campaignId}/numbers`). Check provisioning with `listAssignedNumberStatuses` (`GET .../numbers/status`).
5. **Carrier acceptance**: `getMnoStatus` (`GET /v1/messaging/campaigns/{campaignId}/mno-status`).

## Notes
- Brand and campaign creation submit to TCR, which can bill per submission. Read the resource back before retrying a create.
