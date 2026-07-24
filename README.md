# Awesome AI Agent Accountability [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of standards, regulations, and tools for making AI agent behavior **verifiable and accountable** — proving what an agent *actually did*, not just what it was *allowed* to do.

As AI agents begin to move money, call tools, and make changes on their own, two questions matter: **who is behind the agent** (identity), and **what did it actually do, under what authority, and can anyone prove it** (accountability). Most of the ecosystem ships the first and the "what it's allowed to do" half of the second (governance, guardrails). This list focuses on the missing piece: **evidence** — tamper-evident, independently verifiable records of agent behavior.

Contributions welcome — see [Contributing](#contributing). Listing is by relevance, not endorsement; fixes and additions via PR.

## Contents

- [Concepts](#concepts)
- [Standards & Specifications](#standards--specifications)
- [Regulations](#regulations)
- [Tools & Implementations](#tools--implementations)
- [MCP Governance & Security Servers](#mcp-governance--security-servers)
- [Identity & Provenance](#identity--provenance)
- [Reading](#reading)
- [Contributing](#contributing)

## Concepts

- **Execution / behavioral receipt** — a signed record of a single agent action, verifiable by a third party.
- **Bilateral receipt pattern** — a pair of receipts per action: an *admission* receipt (intent, authority, authorized scope, policy in effect) before execution, and an *outcome* receipt (what happened) after, linked by a content-derived hash. Normative guidance in [OWASP AST09](#standards--specifications).
- **Offline / trustless verification** — checking a record with only the record and a public key, without trusting the operator that produced it.
- **Canonicalization** — deterministic serialization so any implementation produces identical bytes to sign/verify. See [JCS (RFC 8785)](https://www.rfc-editor.org/rfc/rfc8785).
- **Hash-chaining** — linking records so deletion or reordering is detectable as a gap.
- **Identity vs. accountability** — identity binds an agent to a party; accountability proves what the agent did. You need both. ([primer](https://nobulex.com/ai-agent-identity-vs-accountability))

## Standards & Specifications

- [OWASP Agentic Skills Top 10 — AST09: Missing Execution Receipts](https://github.com/OWASP/www-project-agentic-skills-top-10) — the accountability-gap risk class for agentic skills. The bilateral receipt pattern is its normative guidance ([PR #35](https://github.com/OWASP/www-project-agentic-skills-top-10/pull/35)); the concrete `action_ref` construction is in the solutions catalog ([PR #38](https://github.com/OWASP/www-project-agentic-skills-top-10/pull/38)).
- [OWASP CheatSheetSeries](https://github.com/OWASP/CheatSheetSeries) — practical guidance including JCS canonicalization, cross-agent accountability, and regulatory mapping for agent receipts.
- [W3C Verifiable Credentials](https://www.w3.org/TR/vc-data-model-2.0/) & [Decentralized Identifiers (DIDs)](https://www.w3.org/TR/did-core/) — the identity layer agent accountability builds on top of.
- [JCS — JSON Canonicalization Scheme (RFC 8785)](https://www.rfc-editor.org/rfc/rfc8785) — deterministic JSON serialization used to make signed records reproducible across implementations.
- [Ed25519 (RFC 8032)](https://www.rfc-editor.org/rfc/rfc8032) — the signature scheme most receipt implementations use.
- [RFC 3161 — Time-Stamp Protocol](https://www.rfc-editor.org/rfc/rfc3161) & [RFC 6962 — Certificate Transparency](https://www.rfc-editor.org/rfc/rfc6962) — optional anchoring/timestamping for long-term non-repudiation.

## Regulations

- [EU AI Act (Regulation (EU) 2024/1689), Article 12 — Record-keeping](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — requires high-risk AI systems to automatically record events over their lifetime to enable post-hoc traceability. High-risk obligations enforce from Dec 2, 2027. ([mapping](https://nobulex.com/eu-ai-act-article-12-agent-records))
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) — "Measure" and "Manage" functions call for traceability and documentation of AI system behavior.
- [Colorado AI Act (SB 24-205)](https://leg.colorado.gov/bills/sb24-205) — US state-level requirements around documentation and consumer impact of high-risk AI systems.

## Tools & Implementations

*Open-source and commercial tools that produce or verify records of agent behavior.*

- [Nobulex](https://github.com/arian-gogani/nobulex) — Open-source (MIT) reference implementation of the OWASP AST09 bilateral receipt pattern. Signed, hash-chained, offline-verifiable receipts for every agent action; Python (`pip install nobulex`) and TypeScript (`npm install @nobulex/core`) SDKs; six framework integrations. Verify a receipt with just the receipt and a public key — no operator trust.
- [Microsoft Agent Governance Toolkit](https://github.com/microsoft/agent-governance-toolkit) — governance and policy tooling for agent deployments, with an adopters registry.
- [OpenTelemetry](https://opentelemetry.io/) — the de facto standard for traces/metrics/logs; the operator-side observability layer that accountability records complement (traces tell you what your system saw; receipts tell a third party what happened).

## MCP Governance & Security Servers

*Model Context Protocol servers that add policy enforcement, audit, or verification to tool-calling agents. (Discovered via [awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) Security section; descriptions summarized from each project.)*

- [Nobulex](https://github.com/arian-gogani/nobulex) (`@nobulex/mcp-server`) — covenant rule-checking + SHA-256 hash-chained, offline-verifiable audit logs for MCP agents.
- [aegis](https://github.com/Acacian/aegis) — policy-based governance for tool calls: YAML policies, approval gates, risk assessment, audit logging.
- [agentward](https://github.com/agentward-ai/agentward) — least-privilege permission control plane; classifies sensitive data and generates compliance audit trails.
- [agentgraph](https://github.com/agentgraph-co/agentgraph) — trust verification and security scanning for third-party MCP servers with signed attestations (Ed25519/JWS).
- [AgentValet](https://github.com/AgentValet/AgentValet) — identity/credential governance broker issuing scoped, short-lived credentials per agent, with audit log and approval gates.
- [arkforge-mcp](https://github.com/ark-forge/arkforge-mcp) — third-party certifying proxy: signs any HTTP call with an independent Ed25519 signature, RFC 3161 timestamp, and Sigstore Rekor anchor.

## Identity & Provenance

*Identity answers "who is behind the agent" — the complementary half of accountability. ([why you need both](https://nobulex.com/ai-agent-identity-vs-accountability))*

- [Concordium](https://www.concordium.com/) — identity-focused approach using zero-knowledge credentials to verify a real party stands behind an agent.
- [Sigstore](https://www.sigstore.dev/) — keyless signing and a transparency log (Rekor); widely used for software supply-chain provenance and applicable to agent artifacts.
- [C2PA](https://c2pa.org/) — content provenance standard; relevant where agents produce or handle media.

## Reading

- [Why evidence, not just governance, for AI agents](https://nobulex.com/why)
- [How to build a verifiable AI agent audit trail](https://nobulex.com/ai-agent-audit-trails)
- [OWASP AST09: execution receipts for AI agents](https://nobulex.com/owasp-ast09-execution-receipts)

## Contributing

Additions and corrections welcome. Open a PR that:

1. Adds the item to the most relevant section, in the existing format (name → link, then a concise, factual one-line description).
2. Keeps descriptions neutral and accurate — no marketing superlatives.
3. Links to a real, working project, standard, or regulation.

Listing here is not an endorsement, and inclusion of a project does not imply it is complete or production-ready. Fixes to any description are welcome from the projects themselves.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related rights to this work.
