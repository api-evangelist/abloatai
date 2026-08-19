---
name: Commit a batch of changes atomically
description: Apply several row operations as one atomic commit with durable premises, so a multi-row agent decision either lands whole or not at all.
api: openapi/abloatai-api-openapi.yml
operations: [commit, getCommit, listCommits, listLogEntries, getLogDelivery]
generated: '2026-08-19'
method: generated
source: openapi/abloatai-api-openapi.yml + https://docs.abloatai.com/guarantees + https://docs.abloatai.com/concurrency-convention
---

# Commit a batch atomically

When an agent's decision touches more than one row, doing it as N separate writes means N chances to half-apply. `commit` is the operation that makes it one.

Base URL: `https://api.abloatai.com/api`. Auth: `Authorization: Bearer $ABLO_API_KEY`.

## Commit

`POST /v1/commits` → `commit`

*"Commit a batch of operations atomically, and/or register durable premises."*

Send `Idempotency-Key` as a **request header** on this operation (max 255 characters) — note that this differs from the model write operations, which take `idempotencyKey` in the body. Derive it from the business event.

Two things go in the body:

- **the operations** — the row changes to apply as one unit.
- **the premises** (`reads` / `track`, up to 500 entries each) — the state this batch depends on. Each names a `model` + `id` (or a `group`), the `readAt` stamp it was read at, and an `onStale` policy of `reject`, `overwrite` or `notify`.

Premises are the point. A commit that says "apply these five changes, and only if these eight rows are still as I read them" is a transaction over state you do not own, which is not something a plain REST API can express.

## Confirm

`GET /v1/commits/{id}` → `getCommit` returns the commit record. `GET /v1/commits` → `listCommits` lists them.

A returned receipt means committed — the write is in the ordered log and will reach your Postgres and your subscribers. Read `https://docs.abloatai.com/guarantees` for exactly what that promise covers.

## Verify it actually reached someone

`GET /v1/logs` → `listLogEntries` tails what changed and who changed it.

`GET /v1/logs/delivery` → `getLogDelivery` answers the question the other checks can all be green through: *how much of what was recorded could reach anyone*. A change Ablo cannot route is excluded from delivery and counted when it happens. If you wrote rows into Postgres outside Ablo they carry no tenancy value, nothing can route them, and this is where that shows up.

## Rules

- **Only successful requests are metered.** A rejected commit costs nothing.
- **A retry with the same `Idempotency-Key` is metered once**, and — if the original succeeded — replays rather than re-applying.
- **A retry after a failure re-runs.** Failures are not recorded, so the same key executes fresh.
- **`source_transport_pinned` (409)** means the first attempt went through a different route (direct data source vs endpoint fallback). Restore the original route and retry the same key; do not switch.
- **Batch size**: `reads` and `track` cap at 500 entries each.

## Errors

`AbloStaleContextError` — a premise moved. Re-read the rows, regenerate the decision, and write under a **new** key: a new premise set is a new intention, not a retry.
