# Updating an existing spec

Use this for a spec amendment, including implementation discoveries. A
specification correction is required when implementation behavior and the
normative document disagree; do not silently let code become the standard.

## First classify the change

- **Clarification:** the intended interoperable behavior is already clear, but
  wording, examples, links, or identifiers are inaccurate.
- **Security correction:** an ambiguity or omission can widen authority,
  overcharge, replay, leak facilitator funds, or make a valid payment fail.
- **Semantic change:** clients, servers, facilitators, or chain profiles must
  behave differently. Treat this as a protocol design change, not editorial
  cleanup.

State the category, root cause, affected roles, compatibility impact, and
whether existing payloads remain valid.

## Required amendment content

- Replace vague wording with observable, testable MUST/MUST NOT behavior.
- Correct examples and validate them as JSON or the actual wire encoding.
- Specify both `verify` and `settle` consequences, including retries and
  concurrent attempts.
- Add a regression vector or test for the implementation discovery. Include
  malicious and malformed input where relevant.
- Update every coupled spec location: scheme overview/profile, extension,
  transport, core example, docs, and supported-capability advertisement.

## Common triggers

- A verify check permits a transaction that cannot settle, or fails to protect
  the server from invalid work.
- An amount, recipient, asset, network, signature, expiry, nonce, or executor
  is not bound tightly enough.
- A cache, voucher, or ledger sequence behaves incorrectly under retries,
  failures, or multiple replicas.
- A server-provided hint is being trusted for an authoritative client decision.

## Evidence behind these rules

Post-implementation corrections hardened exact amount equality (#1388),
Starknet settlement (#3126), XRPL duplicate settlement (#2801), Hedera
delegated execution (#3205), and SVM batch lifecycle behavior (#2698).
