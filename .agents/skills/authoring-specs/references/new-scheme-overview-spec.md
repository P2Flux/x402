# New scheme overview spec

Use this for `specs/schemes/<scheme>/scheme_<scheme>.md`. A scheme defines
payment semantics shared by all network profiles; it does not define headers,
SDK APIs, or chain-specific transaction encoding.

## Required design work

- Explain the user-visible promise: exact transfer, authorization up to a cap,
  capture/refund, or another distinct settlement semantic. A different
  transaction construction method alone is not a new scheme.
- Define the lifecycle from `PaymentRequirements` through
  `PaymentPayload`, `verify`, `settle`, success, failure, retry, and expiry.
  Name the party that owns every state transition.
- State the authorization and loss boundary precisely. Specify what the client
  authorizes, who can spend it, recipient/amount constraints, and which
  conditions permit a refund, void, partial settlement, or repeated use.
- Define shared invariants for all network profiles: scheme and network
  binding, asset and amount interpretation, timeout behavior, replay behavior,
  idempotency, and the required strength of verification.
- Keep wire data that varies by profile inside `extra`; each profile must
  conform to this overview without changing core type requiredness.

## Authoring checks

- Use transport-neutral terms such as `PaymentPayload` and
  `SettlementResponse`, not HTTP headers.
- Write a normative rule for every security-critical check. Do not leave
  recipient, amount, asset, signature authority, expiry, replay, or settlement
  finality to an implementation note.
- Distinguish one-shot proofs, in-flight retries, settled duplicates, pending
  settlement, and intentionally multi-use authorization. Specify proof
  consumption for each.
- Specify an error or rejection outcome for violated invariants. A verifier
  that accepts a payload but leaves predictable settlement failure unaddressed
  wastes server work.
- Include an example lifecycle and at least one adversarial example: altered
  amount/recipient, replay, expired authorization, and interrupted settlement.

## Evidence behind these rules

Reviews retained `exact` as one scheme despite Permit2 transfer mechanics
(#769), required distinct auth-capture semantics rather than overloaded
metadata (#1425), and treated pending settlement, replay, and retries as
protocol behavior (#3083, #3145). They also repeatedly rejected
transport-specific wording in scheme specifications (#1455, #2741).
