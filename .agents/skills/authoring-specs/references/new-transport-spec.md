# New transport spec

Use this for `specs/transports-v2/<transport>.md`. A transport carries the
core x402 messages; it does not redefine scheme behavior or network
transaction semantics.

## What to specify

- Map payment-required signaling, payment-payload transmission, settlement
  response delivery, and error handling onto the transport's native
  primitives.
- Reference the v2 `PaymentRequirements`, `PaymentPayload`, and
  `SettlementResponse` schemas rather than copying or changing them.
- Define serialization, encoding, size limits, duplicate-field behavior,
  ordering where relevant, and complete success/failure examples.
- Explain how clients distinguish payment-required, invalid-payment, pending,
  and completed outcomes without inventing ambiguous success states.
- Preserve all scheme and extension data unchanged; the transport only states
  where the messages appear and how they are encoded.

## Review before proposing

- Validate every example as the actual encoding used by a client and server.
- Check realistic transport constraints, including header/message size,
  request retries, intermediary behavior, and binary/text encoding.
- Keep HTTP-specific terms out of core and scheme specifications, and keep
  transport-specific terms out of `extra`.
- Link only to `x402-specification-v2.md` and v2 transport references.

## Evidence behind these rules

The transport-agnostic v2 refactor (#417) separated representation from
scheme/network logic. Later reviews corrected HTTP wire placement (#2320) and
flagged a chain profile whose prepared transaction could exceed common HTTP
header limits (#2634).
