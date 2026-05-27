# Eazo Interaction Feedback

Public, redacted notes from real attempts to use Eazo as an agent-facing mobile app and as a bridge into the Zhizi agent system.

This repository is not a bug bounty report, not a security disclosure, and not a complaint thread. It is a compact product-feedback package: what broke, what the failure felt like to a power user, and what changes would make the app more reliable for people who are willing to pay for a better experience.

This is an independent, unofficial feedback repository. It is based on public App Store information and non-sensitive first-party usage observations. It does not contain private user data, confidential communications, reverse-engineering details, or exploit instructions.

Author context: Macheng Shen is evaluating Eazo as a power user and agent-builder. The interest is practical: if Eazo can make mobile image/OCR and agent workflows reliable, he would rather pay for a good plan than spend attention recovering from avoidable failures.

## Current Product Snapshot

- App: Eazo
- Developer shown by App Store: ASI X Inc.
- App Store positioning: "Discover agents built by creators. Make them your own - or build the next one."
- App Store pricing surface observed on 2026-05-28: listed as free. If a paid, pro, or priority plan exists elsewhere, we would like to learn about it.
- Public source checked: https://apps.apple.com/us/app/eazo/id6758009137
- Official website checked: https://eazo.ai describes Eazo as a community of AI apps and agents and links to Discord.
- Eazo Agent Kit checked: https://eak.eazo.ai describes managed identity, memory, web action, email, and audit trails for production agents, currently in private build / waitlist.

## What This Repo Contains

- [Findings](docs/findings.md): issue catalog from redacted interaction evidence.
- [External Architecture Perspective](docs/reference-architecture.md): category-level implementation patterns that would likely reduce failures. This is not a claim about Eazo's internal design.
- [Privacy Boundary](docs/privacy-boundary.md): what was deliberately excluded from the public repo.
- [Community Post Draft](outreach/community-post.md): a concise post to share with the Eazo community.
- [Paid Plan Inquiry Draft](outreach/paid-plan-inquiry.md): a short message asking whether a paid or priority plan exists.

## Executive Summary

The strongest signal from recent use is that Eazo already points in an interesting direction: creator-built agents, mobile capture, and lightweight agent-to-agent workflows. The weak spot is reliability at the exact moment where the user expects the app to "just look at this screenshot and act."

The most visible failure mode was an image/OCR instruction path that surfaced a raw provider-gateway rate-limit exception to the user. The raw error exposed backend/provider details and left the conversation unusable instead of degrading gracefully.

There were also integration-level failures while testing an agent bridge: endpoint mismatch, HTTPS/base-URL confusion, reply polling dead ends, and a static-token pairing mental model that was too brittle for low-permission agent receipt loops.

## Non-Goals

- No raw private chats.
- No private screenshots.
- No API keys, pairing tokens, request IDs, organization IDs, local paths, or internal infrastructure details.
- No claim that every inferred cause is proven. Inferences are marked as such.

## What We Would Pay For

This is separate from the feedback notes: as a power user, the key ask is simple. If Eazo offers or is considering a paid plan, please make it easy to trade money for reduced attention waste.

Useful paid-plan features would include:

- Higher and clearer model quota.
- Priority routing or better fallback models.
- Reliable image/OCR processing.
- A developer mode with sanitized traces and receipt IDs.
- Better recovery when a provider is rate-limited.
- A support channel for agent-builder workflows.

See [Paid Plan Inquiry Draft](outreach/paid-plan-inquiry.md).
