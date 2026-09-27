---
name: authoring-specs
description: Guidelines for authoring x402 specification files. Use when writing or proposing a new x402 spec, such as a per-network scheme spec (scheme_<name>_<chain>.md).
---

# Authoring x402 specs

Guidance for writing x402 specification files under `specs/`. Use RFC-2119 keywords for normative statements (MUST / MUST NOT, SHOULD / SHOULD NOT, MAY).

## General rules

These apply to every spec type (scheme, extension). The references below add type-specific detail.

### Naming

- Name schemes and extensions in lowercase, hyphen-separated kebab-case (e.g. `batch-settlement`, `offer-receipt`), never camelCase.

### Protocol version, networks, and units

- Target protocol v2 only: `x402Version: 2`, the `amount` field (not v1's `maxAmount`), and the `PAYMENT-REQUIRED` / `PAYMENT-SIGNATURE` / `PAYMENT-RESPONSE` headers (not v1's `X-PAYMENT` / `X-PAYMENT-RESPONSE`). See the [v1 to v2 migration guide](../../../docs/guides/migration-v1-to-v2.mdx).
- Use canonical CAIP-2 network notation (e.g. `eip155:84532`, not `base-sepolia`).
- Use atomic units for all amounts.

### Wire format

- Be transport agnostic: specify message contents, not how a particular transport carries them.
- Reference core types (`PaymentRequirements`, `PaymentPayload`, `SettlementResponse`) from [`x402-specification-v2.md`](../../../specs/x402-specification-v2.md).
- Every field a spec defines on the wire must be consumed by a downstream role. Do not include human-readable or otherwise purely informational fields.
- Reuse field names, patterns, and conventions established by existing specs instead of coining new ones.

## References

Select the narrowest reference that applies, then apply the general rules above:

- New core protocol behavior or shared types: [references/core-spec-change.md](references/core-spec-change.md).
- New scheme family overview (`scheme_<name>.md`): [references/new-scheme-overview-spec.md](references/new-scheme-overview-spec.md).
- New per-network scheme (`scheme_<name>_<chain>.md`): [references/new-network-scheme-spec.md](references/new-network-scheme-spec.md).
- New extension: [references/new-extension-spec.md](references/new-extension-spec.md).
- New transport: [references/new-transport-spec.md](references/new-transport-spec.md).
- Amendment to an existing spec: [references/updating-an-existing-spec.md](references/updating-an-existing-spec.md).

Write the spec before its implementation. For a new network, begin with a spec-only
PR; do not encode SDK details or a particular transport into a normative scheme.
Use [specs/CONTRIBUTING.md](../../../specs/CONTRIBUTING.md) for file placement and
the matching template.
