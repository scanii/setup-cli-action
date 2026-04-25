# setup-cli-action

GitHub Action that downloads [scanii-cli](https://github.com/scanii/scanii-cli), starts it as a background server on `localhost:4000`, and waits for readiness — so your SDK tests have a local mock API without any credentials or network calls. Learn more at [scanii.com](https://scanii.com).

## Usage

```yaml
- uses: scanii/setup-cli-action@v1
- run: your-test-command
```

## Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `version` | No | `latest` | scanii-cli version to install. Accepts `latest` or a semver string without a leading `v` (e.g., `1.3.0`). |
| `callback-delay` | No | `0` | Duration to delay callback delivery. Accepts Go duration strings (e.g. `500ms`, `1s`). Passed through as `--callback-wait` to scanii-cli. |

## Outputs

| Output | Description |
|---|---|
| `version` | The actual version installed (resolved from `latest` when applicable). |
| `endpoint` | Always `http://localhost:4000`. Exposed so consumers can reference `${{ steps.setup.outputs.endpoint }}` instead of hardcoding. |

## Worked example

```yaml
- uses: actions/checkout@v4

- name: Setup scanii-cli
  id: scanii
  uses: scanii/setup-cli-action@v1

- name: Run SDK tests
  run: mvn -B verify
  env:
    SCANII_ENDPOINT: ${{ steps.scanii.outputs.endpoint }}
    SCANII_KEY: key
    SCANII_SECRET: secret
```

The server starts with credentials `key` / `secret`. These are scanii-cli's fixed test credentials — they are not real Scanii API keys.

## Lessons inherited from scanii-java

Two non-obvious issues were found while hardening the equivalent hand-rolled bash in scanii-java and are handled automatically by this action:

**Authorization on the GitHub API latest-release endpoint.** Unauthenticated calls to `https://api.github.com/repos/scanii/scanii-cli/releases/latest` return 403 on macOS runners due to per-runner-IP rate limits. The action sends `Authorization: Bearer ${{ github.token }}` on every version-resolve call.

**GoReleaser `wrap_in_directory: true`.** The `sc` binary is not at the root of the downloaded archive. It lives one level deep inside `scanii-cli-{VERSION}-{OS}-{ARCH}/sc`. The action locates it in that subdirectory before adding it to `$GITHUB_PATH`.

## Compatibility

Works on `ubuntu-latest`, `macos-latest`, and `windows-latest`. Requires scanii-cli >= 1.3.0.

This is a composite action — no JavaScript, no Docker, no build step.

## License

Apache 2.0 — see [LICENSE](LICENSE).
