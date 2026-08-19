---
name: Mint a scoped, expiring credential for an agent
description: Hand an agent or a browser its own short-lived, narrowly-scoped credential instead of sharing the backend secret key — mint, inspect, rotate, revoke.
api: openapi/abloatai-api-openapi.yml
operations: [mintEphemeralKey, mintCapability, getCapability, rotateCapability, revokeCapability, createBranch, mintBranchCredential, deleteBranch]
generated: '2026-08-19'
method: generated
source: openapi/abloatai-api-openapi.yml + https://docs.abloatai.com/api-keys + https://docs.abloatai.com/sessions
---

# Mint a scoped credential for an agent

Never ship the backend `sk_` to an agent runtime or a browser. Mint something smaller and expiring.

Base URL: `https://api.abloatai.com/api`. Mint from your server, holding the `sk_`.

## Which credential

| Recipient | Class | Mint with |
|---|---|---|
| A browser reading only | `pk_` | Nothing — a publishable key is safe to ship in the bundle. |
| A browser acting as a logged-in user | `ek_` | `mintEphemeralKey` |
| Another runtime or an agent with a delegated scope | `rk_` | `mintCapability` |
| A throwaway plane for a branch or a CI run | branch-bound `sk_` | `createBranch` + `mintBranchCredential` |

The prefix names the credential's **capability class**, not its target. What it can reach is decided by the server-side key row and an immutable branch binding.

## Short-lived user or agent session

`POST /v1/ephemeral_keys` → `mintEphemeralKey` — *"Mint a short-lived session credential."* Returns an `ek_`, scoped by the typed `can` grant you mint it with (model + verb, e.g. `{ records: ['read','update'] }`) and bounded by its expiry.

## Delegated capability for a system

`POST /v1/capabilities` → `mintCapability` — *"Mint a capability for an agent or system."*

- `GET /v1/capabilities/{id}` → `getCapability` — inspect what a capability can do.
- `POST /v1/capabilities/{id}/rotate` → `rotateCapability` — mint a replacement **keeping the grant**. Use this on schedule, and immediately on suspected exposure.
- `DELETE /v1/capabilities/{id}` → `revokeCapability` — kill it.

## An isolated plane for a test or a CI run

`POST /v1/branches` → `createBranch` (takes an `Idempotency-Key` header) creates an isolated child branch. `POST /v1/branches/{id}/credentials` → `mintBranchCredential` mints its *expiring branch-bound test credential*. `GET /v1/branches/{id}/status` → `getBranchStatus` diagnoses it. `DELETE /v1/branches/{id}` → `deleteBranch` removes the branch **and revokes its credentials**.

The returned branch `id` is immutable — retain that for automation, not the slug.

There is no shared sandbox mode on Ablo. A branch *is* the sandbox, and Production is the protected root of the same tree.

## Rules an agent must not get wrong

- **A mint returns the plaintext exactly once.** Only a hash is kept. Nothing can hand it back to you later — not the API, and deliberately not any MCP tool. Capture it at mint time or mint again.
- **Never write a minted key into a transcript or a log.** It outlives any revocation you issue afterwards.
- **Least privilege.** An `sk_` with an *empty* scope set has full org authority — that is the default for a backend key, and it is not what an agent should hold. Give an agent an `rk_` or `ek_` with an explicit `can` grant.
- **`organization:act-as` is a dedicated minting credential.** Grant it alone, with no data or schema scopes, keep it in a secret manager, and log the target `organizationId`, the minted session id and the request id for audit.
- **Branch binding beats scopes.** A temporary child key can act only inside that child; it cannot manage siblings or reach root, even with no scope strings.

## Errors

`apikey_expired`, `apikey_revoked`, `apikey_invalid`, `capability_invalid`, `capability_scope_denied`, `branch_scope_denied`, `wide_scope_forbidden` — all 401/403 and none retryable. The specific code is also echoed on the CORS-exposed `X-Auth-Failure` response header, so a browser client can branch on it without reading the body.
