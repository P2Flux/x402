# New extension spec

Use this for a capability that enriches x402 without changing a scheme's
payment semantics. Place it under `specs/extensions/` and begin with the
problem that the core scheme cannot solve.

## Boundaries

- Do not alter `accepts`, core types, or scheme selection to carry extension
  data. Define the extension's wire object in `extensions` and say exactly
  where it is offered, returned, and consumed.
- Extension data is optional unless its presence is explicitly negotiated.
  Define absent, unknown, malformed, expired, and unsupported behavior.
- Reuse the protocol's version, network, amount, and identity fields. Add
  fields only when a downstream role consumes them to make a protocol decision.
- Do not use an extension to create a hidden payment mechanism. Move a
  materially different authorization or settlement lifecycle into a scheme.

## Security and interoperability checklist

- Bind signatures, receipts, offers, and identifiers to the correct request,
  origin, network, payer/payee, asset, amount, and expiry. Define the
  canonical bytes or structured-data domain used for that binding.
- Treat server-provided extension values as untrusted. A client must obtain
  authoritative asset metadata and other security-sensitive facts from their
  authoritative source.
- State who is authorized to sign or assert every field. Do not report an
  asserted identity as verified after a failed verification or settlement.
- Define cross-network behavior explicitly. A signature domain or extension
  network must not accidentally follow the payment network when the extension
  has a different authority.
- Supply positive and negative vectors for missing fields, wrong origin,
  wrong network, stale data, unauthorized signer, and cross-request replay.

## Evidence behind these rules

Offer-and-receipt review kept extension data out of `accepts`, corrected
network/domain binding, and rejected reuse of core timeout fields (#935).
SIWX later tightened origin binding (#2859, #3133), and builder-code changes
made authoritative requirement/payload matching explicit (#2050, #3313).
