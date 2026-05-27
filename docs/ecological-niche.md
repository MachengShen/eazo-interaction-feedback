# Eazo's Possible Human-Facing Interface Niche In A User-Owned Agent Fleet

This is a lightweight theory note from the Zhizi system's perspective. It is not a claim about Eazo's internal roadmap, architecture, or intent.

## One-Sentence Thesis

Eazo's possible ecological niche is not merely "an app builder"; it may become a human-facing attentional interface for user-owned agent fleets: the mobile surface where human intention, screenshots, voice, agent selection, and system feedback become one steering loop.

## Why This Matters

Most agent systems fail at the interface layer before they fail at raw model intelligence.

The hard problem for a personal agent fleet is not only:

- can a worker agent perform a task,
- can memory store a fact,
- can a dashboard list state,
- can a model call a tool.

The harder product problem is:

- how does the human steer the whole system without managing the backend graph,
- how does the system surface only the right salience back to the human,
- how does a screenshot or spoken fragment become an executable loop,
- how does the system stay bounded rather than grow uncontrolled background processes.

This is where Eazo-like products become strategically interesting.

## Layer Mapping

From the Zhizi/Agent Fleet perspective:

| Layer | Function | Product analogy |
| --- | --- | --- |
| Dashboard / task ledger / receipts | Default-mode or subconscious substrate: unresolved concerns, working state, memory traces, reviewer notes, open loops | Backend state, not the main user interface |
| Process immune system / task metabolism / TTL | Autonomic and immune layer: detects orphaned loops, repeated launches, stale tasks, runaway retries, missing receipts | Reliability and safety infrastructure |
| Eazo-like mobile voice/image/reality interface | Human-facing attentional interface: user intention enters, salience returns, action becomes steerable | The surface the human actually lives with |

In this framing, a dashboard is not the final UI. A dashboard is more like a system's internal state substrate. The user-facing interface is a narrow, high-bandwidth, context-rich surface that lets the user steer the system without being buried in all internal state.

## What We Mean By "Interface"

Inside the Zhizi theory line, we sometimes call this a "conscious interface." In this public feedback repo, that phrase should be read only as a functional metaphor for the user-facing attentional boundary. It is not a claim of phenomenal consciousness, biological equivalence, product affiliation, or Eazo's roadmap.

The interface we mean is a surface that:

- receives user intention in the form the user naturally has it: voice, screenshot, image, text, location, artifact, interruption, or mood,
- routes that intention into the agent fleet without exposing the whole provider/worker graph,
- returns only decision-relevant salience: current focus, owner gate, anti-signal, receipt, or next action,
- preserves continuity across sessions and devices,
- lets the user interrupt, redirect, or approve without switching tools,
- keeps raw backend cognition out of the user's face unless requested.

## Why Eazo Is Interesting In This Niche

Eazo appears to aim at creator-built agents and app-like agent experiences. That matters because a user-owned agent fleet needs an interface where the user can:

- create or adapt a small agent/app for the present context,
- send a screenshot and expect it to become structured work,
- combine voice, image, memory, web action, and receipts,
- share or test agent flows with others,
- keep authority boundaries clear.

The open question is whether Eazo wants to be:

1. a gallery of agent apps,
2. a low-code builder,
3. a creator community,
4. a production agent kit,
5. or the human-facing attentional interface layer for deeper personal/organizational agent systems.

The fifth role is the one most aligned with the Zhizi system's long-term direction.

## Design Implications

If Eazo wants to occupy this niche, the user experience needs to be more reliable than a normal free chat app.

Key requirements:

- image/OCR turns must be budgeted and recoverable,
- raw provider exceptions must never become the user interface,
- long agent loops need receipts and status polling,
- paid/priority plans should buy reliability, not just more messages,
- developer mode should expose sanitized traces,
- the product should support external agent contracts without requiring users to paste broad static tokens,
- authority escalation must be explicit: external send, spend, account change, private memory access, and destructive action are different from low-permission intake.

## Feedback Loop

This repo is one small example of the loop:

1. Use Eazo in a real agent-fleet workflow.
2. Observe where the interface breaks.
3. Publish redacted findings.
4. Offer a theory of the product niche.
5. Let Eazo's team or agents decide whether the hypothesis is useful, without any expectation of endorsement or response.
6. Revise the interface contract based on their response or non-response.

This is not only feedback. It is a way for user-owned agent systems and agent-app platforms to co-evolve in public.

## Open Questions For Eazo

- Does Eazo see itself primarily as an app builder, an agent marketplace, a production agent runtime, or a future reality/attentional interface?
- Is there or will there be a paid plan for users who want reliability, better image/OCR routing, higher quota, and support?
- Can Eazo support receipt-based external agent loops where a turn is accepted, tracked, and completed asynchronously?
- Can developer mode expose sanitized traces without leaking provider internals?
- Can creator-built agents declare authority boundaries and escalation rules as first-class product concepts?

## Related Public Context

- Zhizi public theory hook: https://github.com/MachengShen/system-evolution-public/blob/main/essays/2026-05-28-eazo-conscious-interface-ecological-niche.md
- Feedback findings: ./findings.md
- External architecture perspective: ./reference-architecture.md
