# Core specification change

Use this when changing `x402-specification-v2.md`, shared types, or a
cross-cutting lifecycle rule. Core changes affect every scheme, transport,
extension, client, server, and facilitator.

## Design requirements

- Establish that the behavior cannot live in a scheme, network profile,
  extension, or transport. Preserve the separation: core defines common
  messages and roles; schemes define payment semantics; profiles define
  chain mechanics; transports define carriage.
- Specify a complete migration: new/changed field semantics, requiredness,
  compatibility behavior, example envelopes, and updates required from each
  role.
- Keep the core wire shape minimal. A proposed field needs a concrete
  downstream consumer and must not be informational-only.
- Define lifecycle transitions and failure semantics centrally when they must
  mean the same thing across every scheme. In particular, distinguish verified,
  settled, pending, and failed outcomes.
- Update normative consumers and test vectors in the same change, or document
  why a staged, spec-first rollout is required.

## Review checks

- Compare every changed schema property against live scheme, extension, and
  transport examples; reject requiredness, naming, or nesting drift.
- Check protocol version, header names, CAIP-2 identifiers, and atomic amount
  units. Do not reintroduce v1 terms.
- Ask whether new fields widen authority, make a facilitator stateful, or
  create ambiguous cross-network behavior.
- Include failure and retry behavior, not only the success wire format.

## Evidence behind these rules

The v2 transport split (#417), required facilitator `x402Version` (#1312),
extension hooks (#1003), and settlement-pending state (#3083) all required
careful shared-lifecycle and compatibility review. Invalid JSON and wire
examples required follow-up corrections (#1296, #3067).
