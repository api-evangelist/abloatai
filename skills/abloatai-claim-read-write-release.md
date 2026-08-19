---
name: Claim, read, write, release a shared row
description: The one safe loop for changing a row that other agents or people may also be editing — acquire a durable claim, read the fresh row, write under the claim, release.
api: openapi/abloatai-api-openapi.yml
operations: [acquireModelClaim, getModelRow, updateModelRow, releaseModelClaim, heartbeatModelClaim]
generated: '2026-08-19'
method: generated
source: openapi/abloatai-api-openapi.yml + https://docs.abloatai.com/coordination + https://docs.abloatai.com/interaction-model
---

# Claim, read, write, release

Ablo's whole point is that two writers touching one row **serialize instead of clobbering**. A plain write is last-write-wins. This is the loop that is not.

Base URL: `https://api.abloatai.com/api`. Every request: `Authorization: Bearer $ABLO_API_KEY`.

`{model}` is a model name from the schema the account pushed — get the real list from `getSchema` first, never guess it.

## 1. Find out what you can address

`GET /v1/schema` → `getSchema`

Returns the models this credential can address, plus its environment and project. If your model name is not in here, every write to it fails with `server_execute_unknown_model` — the schema has not been pushed.

## 2. Claim the row

`POST /v1/models/{model}/{id}/claim` → `acquireModelClaim`

Two success codes, and the difference matters:

- **201** — acquired. You hold the lease. `fenceToken` and `expiresAt` come back with it.
- **202** — queued. Someone else holds it and you are in the wait-line at `position`, behind `heldBy`. Poll `getClaim` (`GET /v1/claims/{claimId}`) until it flips to acquired.

Claims do **not** lock. Waiting is the design: when the holder releases, you are handed the row as it now is, not as it was when you asked.

## 3. Read the fresh row

`GET /v1/models/{model}/{id}` → `getModelRow`

Do this **after** acquiring, not before. The whole reason to wait is that the previous holder probably changed it. The response carries `stamp` — the watermark you read at — and `claims`, the active claims on the row.

## 4. Write under the claim

`PATCH /v1/models/{model}/{id}` → `updateModelRow`

Send:

- `claim` — the claim id you hold. The write is rejected if the row moved underneath you.
- `readAt` — the `stamp` from step 3, so the write declares what it was based on.
- `onStale` — `reject` (default and correct for most agent work), `overwrite`, or `notify`.
- `idempotencyKey` — **derive it from the business event**, e.g. `record:42:mark-done:v1`. Never from `randomUUID()`, a timestamp, or an attempt counter; those regenerate on retry and protect nothing.

## 5. Heartbeat if the work is long

`POST /v1/models/{model}/{id}/claim/heartbeat` → `heartbeatModelClaim`

A lease expires. If your work outlives `expiresAt`, heartbeat before it does, or you get `claim_lost` and someone else has the row. Holding and releasing are free — only creating a claim is metered.

## 6. Release

`DELETE /v1/models/{model}/{id}/claim` → `releaseModelClaim`

Release as soon as you are done, in a `finally`. A held lease is a queue of blocked agents.

## Errors that change what you do

| Code | HTTP | Do |
|---|---|---|
| `claim_queued` | 202 | You are in line. Poll `getClaim`; do not write yet. |
| `claim_conflict` | 409 | Someone else holds it. Wait or yield. |
| `claim_lost` | 409 | Your lease expired mid-work. Re-acquire and re-read before writing anything. |
| `idempotency_conflict` | 409 | Identical retry → the original is in flight, wait and retry the **same** key. Changed request → your bug, use a **new** key. |
| `rate_limit_exceeded` | 429 | Back off for the `Retry-After` delay. |
| `quota_exceeded` | 429 | The org is out of operations. Stop; retrying will not help until the period rolls over. |

Every error body carries `code`, `request_id` and a `doc_url` pointing at the exact anchor in the 288-code registry. Read `code`, not `message`.

## The one thing that surprises people

**A failed write is not replayed — it re-runs.** Only successful writes are recorded against an idempotency key. So after a timeout where you do not know whether the write landed, retry with the **same** key: if it landed you get the recorded success, and if it did not, the write happens now. That is exactly the case the key exists for.
