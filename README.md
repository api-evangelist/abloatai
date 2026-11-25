# Ablo

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Ablo is collaboration infrastructure for AI agents: one API that lets agents, apps and people claim, change and confirm the same database rows without clobbering each other. Rows stay in the customer's own Postgres — Ablo holds only an ordered transaction log and the coordination state — while durable claims (leases with a wait-line, which make a second writer wait and then hand it the fresh row rather than lock), guarded idempotent writes with stale premises, atomic batch commits and signed webhook delivery turn concurrent multi-agent editing into something that serializes instead of racing. The surface is a 32-operation OpenAPI 3.1 REST API, a WSS realtime stream, a TypeScript SDK and CLI released in lockstep with the API, and two MCP servers.

**Areas:** Agent Infrastructure · Multi‑Agent Coordination · Concurrency Control · State Management · Database · PostgreSQL · Real-time Sync · MCP · Developer Tools · Backend Infrastructure

## Ablo on APIs.io

Everything profiled for Ablo is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/abloatai/](https://apis.io/providers/abloatai/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [Ablo Integration-Helper MCP Server](https://apis.io/apis/abloatai/ablo-integration-helper-mcp-server/) — A public, anonymous, stateless Streamable-HTTP MCP server Ablo runs on its own domain so an AI coding assistant writing an Ablo integration can search the…
- [Ablo Branches API](https://apis.io/apis/abloatai/ablo-branches-api/) — The branches API from Ablo — 4 operation(s) for branches.
- [Ablo Claims API](https://apis.io/apis/abloatai/ablo-claims-api/) — The claims API from Ablo — 7 operation(s) for claims.
- [Ablo Commits API](https://apis.io/apis/abloatai/ablo-commits-api/) — The commits API from Ablo — 2 operation(s) for commits.
- [Ablo Credentials API](https://apis.io/apis/abloatai/ablo-credentials-api/) — The credentials API from Ablo — 5 operation(s) for credentials.
- [Ablo Logs API](https://apis.io/apis/abloatai/ablo-logs-api/) — The logs API from Ablo — 2 operation(s) for logs.
- [Ablo Models API](https://apis.io/apis/abloatai/ablo-models-api/) — The models API from Ablo — 2 operation(s) for models.
- [Ablo Schema API](https://apis.io/apis/abloatai/ablo-schema-api/) — The schema API from Ablo — 1 operation(s) for schema.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Abloatai Webhooks](https://apis.io/asyncapis/abloatai/abloatai-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [abloatai-mcp.yml](https://apis.io/servers/abloatai/abloatai-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Ablo Agentic Access](https://apis.io/agentic-access/abloatai/abloatai-agentic-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Ablo Api Overlay](https://apis.io/overlays/abloatai/abloatai-api-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [Ablo API Rules](https://apis.io/rules/abloatai/abloatai-rules/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Ablo Authentication](https://apis.io/security/abloatai/abloatai-authentication/)
- [Ablo Domain Security](https://apis.io/security/abloatai/abloatai-domain-security/)

#### Plans and pricing (2)

Published plans, tiers and prices.

- [Ablo Plans Pricing](https://apis.io/plans/abloatai/abloatai-plans-pricing/)
- [Ablo Price Estimates](https://apis.io/plans/abloatai/abloatai-price-estimates/)

#### Rate limits (1)

Published request limits.

- [Ablo Rate Limits](https://apis.io/rate-limits/abloatai/abloatai-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Ablo Finops](https://apis.io/finops/abloatai/abloatai-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.abloatai.com/)
- [Documentation](https://docs.abloatai.com)
- [Getting started](https://docs.abloatai.com/quickstart)
- [API reference](https://docs.abloatai.com/api-reference)
- [Pricing](https://docs.abloatai.com/pricing)
- [Sign up](https://dashboard.abloatai.com)
- [Support](https://github.com/Abloatai/abloatai/issues)
- [Changelog](https://github.com/Abloatai/abloatai/blob/main/CHANGELOG.md)
- [Blog](https://www.abloatai.com/blog)
- [Terms of service](https://www.abloatai.com/terms-conditions)
- [Privacy policy](https://www.abloatai.com/privacy-policy)
- [GitHub](https://github.com/Abloatai)

## API Evangelist rating

**Kin Score 64.8** — strong · rubric 0.28.1 · scored 2026-11-21

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 57.4 |
| Contract governance | 18.2 |
| Operational transparency | 52.4 |
| Developer ergonomics | 75.6 |
| Access clarity | 75.5 |

**Agent readiness 43.0** — agent-native

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | verified |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | paginated |
| Operation Distinctiveness | distinct |
| MCP Tool Quality | na |
| Onboarding Descriptor | — |
| Dns Aid Index | na |
| Markdown Negotiation | na |

**Regulatory alignment 18.6** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/abloatai/](https://apis.io/providers/abloatai/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Ablo: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/abloatai/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
