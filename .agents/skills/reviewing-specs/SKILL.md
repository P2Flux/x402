---
name: reviewing-specs
description: Review x402 core, scheme, network-profile, extension, and transport specifications using lessons from all merged spec PR discussions.
---

# Reviewing x402 specs

Use this skill for any PR that changes `specs/`, whether it introduces a spec,
amends one after an implementation finding, or combines implementation with a
necessary specification update. Review the normative behavior, not prose
quality alone.

## Review order

1. **Classify the change.** Is it core, scheme overview, network scheme,
   extension, transport, or amendment? A change can span categories, but each
   category needs its own acceptance criteria.
2. **Trace the protocol path.** Start with requirements; then build/sign the
   payload; then verify; then settle; then success, failure, retry, and expiry.
   Identify the client, server, facilitator, and chain decision at every step.
3. **Trace authority and economics.** For every signature, account, executor,
   fee payer, receipt, and server-provided field, state what it can authorize
   and the maximum funds/work/liquidity it can expose.
4. **Check interoperability.** Compare every wire example and field name with
   v2 core types and existing profiles. Reject v1 headers/fields, non-CAIP
   network names, non-atomic amounts, and transport or SDK leakage.
5. **Try to break the lifecycle.** Evaluate malformed input, wrong network or
   asset, altered amount/recipient, replay, duplicate concurrent settle,
   timeout, temporary failure, stale cache, multiple replicas, and resource
   handler failure.

## Non-negotiable principles

- A spec is an interoperability contract. Every MUST needs an observable
  condition, responsible role, and failure behavior.
- `verify` protects the server from doing work that cannot yield a valid
  settlement. It must provide the strongest feasible pre-settlement guarantee,
  normally using simulation or explicit authoritative chain checks.
- The client treats server data as untrusted unless it can independently verify
  it. The server and facilitator treat client payloads as untrusted.
- Canonical capability advertisements may be narrowed downstream but must not
  be widened. Do not imply wildcards, defaults, or fallbacks that are not
  explicitly defined.
- Favor stateless client/server/facilitator designs. State must have an owner,
  key, lifetime, multi-replica behavior, retry behavior, and loss boundary.
- Example payloads are part of the contract: validate them against the actual
  encoding and exercise them in an implementation.

## Type-specific review

### Core

- Confirm the behavior cannot live in a lower layer.
- Check shared-type requiredness, compatibility, complete envelopes, lifecycle
  states, and migration impact on every role.
- Require a consumer for every new wire field and reject informational fields.

### Scheme overview

- Confirm it represents distinct payment semantics, not another chain method.
- Review authorization scope, custody/refund/capture semantics, exactness or
  partial-settlement rules, proof consumption, and cross-profile invariants.
- Require explicit handling for pending, failed, retried, and duplicate
  settlement.

### Network scheme

- Require canonical CAIP-2 IDs, atomic units, authoritative asset identifiers,
  and exact signature/transaction bytes and account types.
- Verify scheme/network/version/asset/amount/recipient/expiry binding.
- Review simulation or preflight, account preconditions, fees, sponsorship,
  nonce/sequence strategy, fee-payer isolation, and settlement finality.
- Validate real transport feasibility (size/encoding) and wallet-produced
  transactions, not only hand-built fixtures.

### Extension

- Ensure it extends `extensions` without changing core `accepts` or embedding
  a new mechanism.
- Check negotiation and unsupported behavior, signature domain/origin/network
  binding, signer authorization, cross-request replay, and authoritative
  source of extension facts.

### Transport

- Check only carriage is specified: signaling, placement, encoding, errors,
  size limits, retry/intermediary behavior, and complete examples.
- Ensure it references v2 core types and does not redefine scheme semantics.

### Amendment driven by implementation

- Require a root-cause statement that explains which normative ambiguity or
  error the implementation exposed.
- Decide whether it is clarification, security fix, or semantic change; verify
  compatibility explicitly.
- Demand the regression test/vector and updates to every coupled spec.

## Evidence from merged PR discussions

This checklist is grounded in the repository's merged spec PR history:

- Transport neutrality and layer separation: #417, #1455, #2741.
- Exact authority, amount, and atomicity: #769, #1388, #1425.
- Verify-before-work and simulation/preflight: #829, #1455, #1575, #2634.
- Fee-payer isolation and bounded delegated execution: #546, #3205.
- Replay, retry, and multi-replica settlement: #1468, #2547, #2698, #2801.
- Core compatibility and authoritative capability data: #1312, #2050, #2698,
  #3205.
- Extension signature/origin/network binding: #935, #2859, #3133.
- Wire-format and example correctness: #1201, #1296, #3067, #3235.

For authoring guidance, use `authoring-specs` and its type-specific references.
