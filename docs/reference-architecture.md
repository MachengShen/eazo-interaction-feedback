# External Architecture Perspective

This is a category-level external perspective for mobile agent apps that accept screenshots, run image/OCR, and call LLM-backed agent loops. It is intentionally product-facing and is not a claim about Eazo's internal implementation.

## 1. Budgeted Image/OCR Pipeline

Recommended flow:

1. Accept image upload.
2. Run image validation: size, type, dimensions, privacy warnings if needed.
3. Run OCR/vision extraction in a bounded preprocessor.
4. Produce a compact structured observation:
   - visible text,
   - UI/app context if detected,
   - user-visible error if present,
   - confidence,
   - uncertain regions.
5. Estimate token cost before the main agent call.
6. Send only the compact observation, the user's goal, and a short conversation summary into the main agent loop.

Failure mode:

- If OCR/vision is rate-limited, show a recoverable state and keep a receipt.
- If the image is too large, ask the user to crop or offer paid/priority processing.

## 2. Provider Error Handling

Provider errors should be classified before reaching the UI.

Suggested categories:

- `provider_rate_limited`
- `model_unavailable`
- `input_too_large`
- `tool_contract_error`
- `network_or_transport_error`
- `internal_unexpected_error`

Each category should map to:

- user-facing message,
- retry policy,
- telemetry event,
- sanitized developer trace,
- whether the turn is saved as a resumable receipt.

## 3. Fallback Routing

The mobile agent loop should not depend on a single model group.

Recommended fallback chain:

- primary high-quality model for normal agent turns,
- cheaper OCR summarizer for image preprocessing,
- smaller fallback for short clarifying questions,
- async queue if no model is currently available.

For provider 429/TPM errors:

- do not immediately replay the entire expensive prompt,
- back off,
- reduce context,
- switch to an alternate model group,
- preserve the user's turn.

## 4. Error Scrubbing

Never show raw backend exceptions in normal chat.

Scrub before UI:

- organization IDs,
- request IDs unless explicitly marked safe,
- provider account metadata,
- model-group internal names,
- stack traces,
- local paths,
- tokens and token-like strings.

Developer mode can show a sanitized trace ID and a "copy diagnostic" button.

## 5. Agent Bridge Contract

For Eazo-to-external-agent integrations, use a receipt contract:

1. `POST /agent-turn` or equivalent accepts a bounded public-safe payload.
2. Response returns `receipt_id`, `status_url`, and clear capability boundary.
3. Client polls status or subscribes to updates.
4. Server stores durable state and never silently drops the turn.
5. Sensitive actions require explicit owner approval.

The app should fetch a live manifest with:

- base URL,
- endpoint paths,
- schema version,
- auth model,
- size/rate limits,
- current production-readiness status.

## 6. Paid/Pro Plan Shape

A paid plan would be valuable if it directly reduces attention waste.

Possible plan features:

- priority image/OCR processing,
- higher token and image quotas,
- better fallback routing,
- retry queue for provider congestion,
- developer diagnostics,
- integration support channel,
- exportable receipts for bug reports.

Avoid selling only "more messages" if the real user pain is reliability.
