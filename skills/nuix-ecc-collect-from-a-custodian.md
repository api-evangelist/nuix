---
name: nuix-ecc-collect-from-a-custodian
description: Stand up an Enterprise Collection Center case, register a custodian and target, launch a
  collection, and monitor the job to completion via its logs.
api: openapi/nuix-case-api-openapi.yml
operations:
- createCase
- createCustodian
- createTarget
- launchCaseCollection
- jobDetails
- listLogByJob
generated: '2026-10-21'
method: generated
source: Grounded in openapi/nuix-case-api-openapi.yml and sibling ECC specs (Custodian, Target, Collection
  and Survey, Job, Log APIs).
---

# Collect data from a custodian with ECC

Stand up an Enterprise Collection Center case, register a custodian and target, launch a collection, and monitor the job to completion via its logs.

## Auth

HTTP Basic (BasicAuth), per the ECC specs.

## Base URL

Self-hosted: use the servers[] entry of the spec for your own installation.

## Steps

1. Create the ECC case: `createCase` (POST /v2/cases, Case API).
2. Register the custodian: `createCustodian` (POST /v2/custodians).
3. Register the collection target: `createTarget` (POST /v2/target).
4. Launch the collection: `launchCaseCollection` (POST /v2/collections). Keep the returned job id.
5. Poll the job: `jobDetails` (GET /v2/jobs/{id}) until it reaches a terminal state.
6. Read the job log: `listLogByJob` (GET /v2/logs).

## Rules

- Treat DELETE, cancel and job-action calls as confirmation-required (see overlays/).
- Long-running work is asynchronous; poll, do not resubmit. The specs document no idempotency keys.
