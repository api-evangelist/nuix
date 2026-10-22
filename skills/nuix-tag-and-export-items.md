---
name: nuix-tag-and-export-items
description: Cull a case by bulk-tagging items that match a query, then export them and monitor the export.
api: openapi/nuix-core-engine-rest-api-openapi.yml
operations:
- openCase
- getTags
- bulkTag
- export
- getAsyncFunctionStatus
generated: '2026-10-21'
method: generated
source: Grounded in openapi/nuix-core-engine-rest-api-openapi.yml.
---

# Tag and export items from a Nuix case

Cull a case by bulk-tagging items that match a query, then export them and monitor the export.

## Auth

ApiKeyAuth or bearerAuth per the spec. A licensed session is acquired through the documented login operation, which a human or the customer's own integration performs; this skill does not call it.

## Base URL

Self-hosted: use the servers[] entry of the spec for your own installation.

## Steps

1. Open the case: `openCase` (GET /cases/{caseId}).
2. Review existing tags: `getTags` (GET /cases/{caseId}/tags).
3. Tag matching items: `bulkTag` (POST /cases/{caseId}/itemTags).
4. Export: `export` (PUT /cases/{caseId}/export).
5. Track the export with `getAsyncFunctionStatus` (GET /asyncFunctions/{key}).

## Rules

- Treat DELETE, cancel and job-action calls as confirmation-required (see overlays/).
- Long-running work is asynchronous; poll, do not resubmit. The specs document no idempotency keys.
