# wristlink-contract

Shared protocol assets for WristLink phone-to-watch messages and
watch-to-phone acknowledgements.

This repository is the source of truth for the Flutter phone app and the
separate Garmin Connect IQ watch app. App repositories consume these schemas,
fixtures, and notes; they do not redefine incompatible message shapes locally.

## Version 1

Version 1 assets live under `protocol/v1/` and `fixtures/v1/`.

- Phone-to-watch messages use `protocol/v1/message.schema.json`.
- Watch-to-phone acknowledgements use
  `protocol/v1/acknowledgement.schema.json`.
- Contract metadata, including the serialized message budget and
  acknowledgement requirements, lives in `protocol/v1/metadata.json`.
- Valid and invalid examples live under `fixtures/v1/`.

The v1 serialized phone-to-watch message budget is **1024 UTF-8 JSON bytes**.
Producers must reject larger envelopes before invoking Garmin transport.
