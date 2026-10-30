# KG-NINJA

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

KG-NINJA is the operator identity (GitHub user KG-NINJA, Osaka; Cloudflare Wallet handle @kgninja) behind Agent Verification Utility at agent-economy.kgninja.dev, a Cloudflare Workers service (v0.4.3) that runs bounded deterministic checks (SHA-256 equality, JSON Pointer existence/equality/type) over caller-supplied inline JSON and returns an Ed25519-signed execution record, sold to agents per call for 10000 atomic USDC ($0.01) on Base through x402 v2 with no account, API key or OAuth. One origin exposes the product three ways — a 44-operation OpenAPI 3.1.0 contract at /openapi.json, a remote Streamable HTTP MCP server at /mcp answering an anonymous tools/list (five tools, one paid), and an A2A 1.0 agent card at /.well-known/agent-card.json with JSON-RPC at /a2a — plus an RFC 9727 API catalog, an MCP Registry server.json, a provider-published Agent Skill, a JWKS, an x402 manifest, llms.txt and Content-Signal. The apex kgninja.dev has no A record; the subdomain is its only web presence.

**Areas:** Agents · Agentic Commerce · A2A · MCP · x402 · Verification · JSON · Cryptography · Cloudflare Workers · Agent-Native · Japan

## KG-NINJA on APIs.io

Everything profiled for KG-NINJA is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/kgninja-dev/](https://apis.io/providers/kgninja-dev/)** — or go straight to one of the APIs or properties below.

### APIs (10)

- [Agent Verification Utility MCP Server](https://apis.io/apis/kgninja-dev/agent-verification-utility-mcp-server/) — Remote Model Context Protocol server at https://agent-economy.kgninja.dev/mcp — stateless Streamable HTTP, POST only (GET returns 405), serverInfo…
- [Agent Verification Utility A2A Agent](https://apis.io/apis/kgninja-dev/agent-verification-utility-a2a-agent/) — Agent2Agent (A2A) 1.0 surface: an agent card served from https://agent-economy.kgninja.dev/.well-known/agent-card.json (supportedInterfaces[] JSONRPC at…
- [KG-NINJA A2a API](https://apis.io/apis/kgninja-dev/kgninja-dev-a2a-api/) — Synchronous A2A 1.0 JSON-RPC quote preparation
- [KG-NINJA Decision Support API](https://apis.io/apis/kgninja-dev/kgninja-dev-decision-support-api/) — Free use-case selection and request validation before payment
- [KG-NINJA Discovery API](https://apis.io/apis/kgninja-dev/kgninja-dev-discovery-api/) — Machine-readable service discovery
- [KG-NINJA Goals API](https://apis.io/apis/kgninja-dev/kgninja-dev-goals-api/) — Goal-driven revenue measurement and improvement recommendations
- [KG-NINJA MCP API](https://apis.io/apis/kgninja-dev/kgninja-dev-mcp-api/) — Stateless MCP Streamable HTTP transport
- [KG-NINJA Operations API](https://apis.io/apis/kgninja-dev/kgninja-dev-operations-api/) — Availability and operational status
- [KG-NINJA Purchase API](https://apis.io/apis/kgninja-dev/kgninja-dev-purchase-api/) — Quote and paid verification operations
- [KG-NINJA Registration API](https://apis.io/apis/kgninja-dev/kgninja-dev-registration-api/) — Optional anonymous, short-lived agent registration

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [kgninja-dev-mcp.yml](https://apis.io/servers/kgninja-dev/kgninja-dev-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Kgninja Dev Agentic Access](https://apis.io/agentic-access/kgninja-dev/kgninja-dev-agentic-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Kgninja Dev Openapi Overlay](https://apis.io/overlays/kgninja-dev/kgninja-dev-openapi-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Kgninja Dev Authentication](https://apis.io/security/kgninja-dev/kgninja-dev-authentication/)
- [Kgninja Dev Domain Security](https://apis.io/security/kgninja-dev/kgninja-dev-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Kgninja Dev Plans Pricing](https://apis.io/plans/kgninja-dev/kgninja-dev-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Kgninja Dev Rate Limits](https://apis.io/rate-limits/kgninja-dev/kgninja-dev-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://agent-economy.kgninja.dev/)
- [Documentation](https://agent-economy.kgninja.dev/docs/deterministic-json-verification)
- [Getting started](https://agent-economy.kgninja.dev/index.md)
- [API reference](https://agent-economy.kgninja.dev/openapi.json)
- [Pricing](https://agent-economy.kgninja.dev/agent.json)
- [GitHub](https://github.com/KG-NINJA)

## API Evangelist rating

**Kin Score 37.7** — thin · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 42.2 |
| Contract governance | 18.2 |
| Operational transparency | 5.3 |
| Developer ergonomics | 54.8 |
| Access clarity | 31.6 |

**Agent readiness 53.9** — agent-native

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | verified |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | verified |
| Event Surface Described | — |
| Agent Skills | yes |
| Well Known Catalog | yes |
| Consent Identity | yes |
| Agent Card | conformant |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 16.7** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/kgninja-dev/](https://apis.io/providers/kgninja-dev/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for KG-NINJA: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/kgninja-dev/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
