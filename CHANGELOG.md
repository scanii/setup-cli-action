# Changelog

## 1.0.1 — 2026-04-29

- Fix: detach `sc server` from the parent shell on Windows. MSYS2 bash does not properly
  detach native Windows processes with bare `&`; when the shell exits the step, sc.exe
  inherits closed stdio and dies on its first write. Fix: redirect stdout/stderr to
  `$RUNNER_TEMP/scanii-cli.log`, add `nohup`, and `disown` the job. The log file is also
  surfaced on the timeout path to ease future diagnostics.

## 1.0.0 — 2026-04-25
Initial release.

- Composite action: no JavaScript, no Docker, runs natively on `ubuntu-latest`, `macos-latest`, and `windows-latest`
- Downloads scanii-cli at a specified or latest version
- Starts `sc server` as a background process on `localhost:4000`
- Polls `/v2.2/ping` for up to 30 seconds and fails loudly if the server never becomes ready
- Exposes `version` and `endpoint` outputs
- Supports `callback-delay` input for async-scan test flows
