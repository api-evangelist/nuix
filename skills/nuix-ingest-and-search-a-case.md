---
name: nuix-ingest-and-search-a-case
description: Create a Nuix Engine case through the REST service, ingest evidence, wait for the async processing
  job, then search and count items.
api: openapi/nuix-core-engine-rest-api-openapi.yml
operations:
- createCase
- bulkIngestionIntoSimpleCase
- getAsyncFunctionStatus
- searchNativeResult
- count
generated: '2026-10-21'
method: generated
source: Grounded in openapi/nuix-core-engine-rest-api-openapi.yml.
---

# Ingest evidence and search a Nuix case

Create a Nuix Engine case through the REST service, ingest evidence, wait for the async processing job, then search and count items.

## Auth

ApiKeyAuth or bearerAuth per the spec. A licensed session is acquired through the documented login operation, which a human or the customer's own integration performs; this skill does not call it.

## Base URL

Self-hosted: use the servers[] entry of the spec for your own installation.

## Steps

1. Create the case: `createCase` (POST /cases).
2. Ingest evidence: `bulkIngestionIntoSimpleCase` (POST /cases/{caseId}/evidence), or a source-specific variant such as `ingestS3IntoSimpleCase`.
3. Processing is asynchronous: poll `getAsyncFunctionStatus` (GET /asyncFunctions/{key}) with the returned key.
4. Search: `searchNativeResult` (GET /cases/{caseId}/search) with a Nuix query string; page with startIndex / numberOfRecordsRequested.
5. Size a result set without paging: `count` (GET /cases/{caseId}/count).

## Rules

- Treat DELETE, cancel and job-action calls as confirmation-required (see overlays/).
- Long-running work is asynchronous; poll, do not resubmit. The specs document no idempotency keys.
