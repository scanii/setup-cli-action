# Changelog

## 1.0.0 — 2026-04-25

Initial release.

- Composite action: no JavaScript, no Docker, runs natively on `ubuntu-latest`, `macos-latest`, and `windows-latest`
- Downloads scanii-cli at a specified or latest version
- Starts `sc server` as a background process on `localhost:4000`
- Polls `/v2.2/ping` for up to 30 seconds and fails loudly if the server never becomes ready
- Exposes `version` and `endpoint` outputs
- Supports `callback-delay` input for async-scan test flows
