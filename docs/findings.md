# Findings

These findings come from redacted interaction notes, local receipt logs, public app metadata, and sanitized observations from screenshots. Exact private identifiers are intentionally omitted.

Evidence labels:

- Public: based on public app/store/web material.
- Firsthand: observed directly during our own product usage.
- Inference: plausible diagnosis from observed behavior; not a claim about Eazo's private implementation.
- External-system: lessons from our own bridge/relay experiments, included only when relevant to agent-app design.

## F1. Image/OCR Instructions Can Fail the Whole Agent Loop

Evidence basis: Firsthand + inference.

Observed behavior:

- A screenshot was sent to Eazo with the expectation that the app would OCR or visually interpret the instruction.
- Instead of a useful answer, the UI displayed a backend exception from the model gateway.
- The exception indicated a provider-gateway rate-limit error from an LLM-backed route.

Likely cause:

- The image/vision path appears to route through the same main mobile-agent loop as ordinary chat.
- Image requests can be token-expensive.
- If the app also resends long conversation history or prior error text, one image turn can become much more expensive than the user expects.

Suggested fix:

- Split image intake into a budgeted preprocessing stage:
  1. receive image,
  2. run OCR/vision extraction,
  3. compress into a short structured observation,
  4. pass only the short observation plus the user goal into the main agent loop.
- Add a token budget check before model dispatch.
- If budget is exceeded, ask a focused clarification instead of attempting a doomed provider call.

## F2. Raw Backend Exceptions Are Exposed in the Frontend

Evidence basis: Firsthand.

Observed behavior:

- The user saw raw provider and gateway details in the chat UI, including model-group and provider-error structure.

Impact:

- The user experience feels broken and untrustworthy.
- Internal details that are useful for developers are too noisy for normal users.
- Some raw exception fields can reveal sensitive operational metadata.

Suggested fix:

- Add a frontend error boundary and backend error scrubber.
- Map provider failures into product states:
  - "The image model is busy. We saved your request; retrying shortly."
  - "This image is too large for the current plan. Try cropping or upgrading."
  - "The agent could not process this turn. Tap to send diagnostic feedback."
- Keep sanitized trace IDs in developer mode only.

## F3. Provider Rate Limits Have No Graceful Degradation

Evidence basis: Firsthand + inference.

Observed behavior:

- The error indicated an input-token-per-minute limit.
- The gateway reported no fallback model group.
- The app retried only once and then failed.

Suggested fix:

- Add fallback model groups for the mobile agent loop.
- Use exponential backoff for 429/TPM errors.
- Use cheaper or lower-context fallback for OCR summarization.
- Preserve the user turn as a receipt so it can complete asynchronously instead of disappearing into an error bubble.

## F4. Conversation State May Be Replayed Too Aggressively

Evidence basis: Inference.

- After a simple follow-up message, the same class of backend error appeared again.
- This suggests the agent loop may be resending too much prior state, possibly including long raw error text.

Suggested fix:

- Never append long backend exception strings into the model-visible conversation history.
- Store exceptions in telemetry, not in user-visible chat memory.
- Maintain a compact state summary separate from raw UI transcript.

## F5. Endpoint Contract Drift Broke Agent Bridge Experiments

Evidence basis: External-system.

Observed behavior:

- During Eazo/Zhizi bridge testing, app-side requests followed an older public handoff contract while the relay initially exposed a different endpoint set.
- The result was 404/unsupported-route style failures until compatibility endpoints were added.

Suggested fix:

- Version the agent tool manifest.
- Let the app fetch a live manifest before calling bridge tools.
- Validate payloads with explicit schema errors.
- Keep old endpoint aliases for at least one release cycle.

## F6. HTTPS and Base-URL Confusion Hurt Mobile Reliability

Evidence basis: External-system.

Observed behavior:

- Early bridge testing used an HTTP staging relay.
- The mobile app path needed HTTPS.
- A later HTTPS staging endpoint resolved this, but it showed how easy it is for an app to keep an old base URL or build a relative status URL incorrectly.

Suggested fix:

- App should display the active base URL in developer mode.
- Status URLs returned by APIs should be absolute or resolved consistently against the manifest base URL.
- Health checks should test the same transport the app will use.

## F7. Reply Polling Needs a Closed Loop

Evidence basis: External-system.

Observed behavior:

- A receipt path worked before the outbox/reply pointer was available.
- From the user's perspective, "submitted" without a visible reply path still feels stuck.

Suggested fix:

- Every agent turn should show:
  - receipt created,
  - current processing state,
  - next retry time or poll state,
  - whether a reply is available,
  - how to report the failure if it stalls.

## F8. Static Pairing Tokens Are a Poor Default UX for Low-Permission Agent Loops

Evidence basis: External-system.

Observed behavior:

- Early pairing experiments treated a static token as the app-side bridge key.
- That created confusion: users were tempted to paste tokens into a mobile UI, while the safer flow was low-permission, tokenless intake with dynamic grant/demotion.

Suggested fix:

- For public-safe low-authority intake, avoid asking users to paste shared secrets.
- Use dynamic grants based on interaction context and payload risk.
- Escalate sensitive actions to explicit owner confirmation instead of trying to encode authority in one copied token.

## F9. The Free Experience Wastes Attention for Power Users

Evidence basis: Firsthand product experience + public App Store pricing surface.

Observed behavior:

- The app is interesting enough to keep testing.
- The free-path reliability is poor enough that failures consume disproportionate attention.

Suggested paid-plan features:

- Higher quota and transparent quota state.
- Priority routing during provider congestion.
- Reliable OCR/image turns.
- Sanitized developer diagnostics.
- Human-readable failure recovery.
- A clear support path for agent builders and integration testers.
