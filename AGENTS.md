# AGENTS.md

## WristLink Message Contract

This submodule is the shared source of truth for WristLink phone-to-watch
messages and watch-to-phone acknowledgements. The Flutter phone app and the
separate Garmin Connect IQ app should consume these protocol assets instead of
redefining incompatible message shapes locally.

## Contract Ownership

- Treat `contract/` as read-only from consuming repositories except when making
  an intentional contract change that will be committed and pinned by each
  parent repository.
- Keep schemas, metadata, fixtures, and protocol notes together for each
  contract version.
- When adopting a changed contract in a parent repository, update that parent's
  submodule pointer and document the adopted contract revision in implementation
  notes or the PR description.
- Do not add Flutter app code or Connect IQ watch app code to this submodule.

## Version 1 Rules

- WristLink v1 uses compact JSON envelopes with protocol version `1`, a
  26-character ULID id, kind, UTC creation timestamp, optional TTL seconds, and
  kind-specific payload data.
- Enforce the v1 serialized phone-to-watch envelope budget of 1024 UTF-8 JSON
  bytes before queueing or invoking Garmin transport.
- Message kinds are `point`, `timer`, `note`, and `command`.
- Watch acknowledgements must reference the original message id and use
  `accepted`, `rejected`, `unsupported`, or `retryable` status.
- Only message kinds whose metadata requires acknowledgement should wait for a
  watch acknowledgement before final send status transitions.

## Verification Expectations

- Update schemas, metadata, valid fixtures, invalid fixtures, and examples
  together when changing message shapes.
- Keep fixture examples compact enough to satisfy the v1 byte budget unless a
  fixture intentionally documents an invalid oversized payload.
- Both consuming repositories should validate their parser/serializer behavior
  against the same fixtures before relying on a payload shape.
