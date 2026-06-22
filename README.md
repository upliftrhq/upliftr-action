# Upliftr — GitHub Action

Gate your pull requests on [Upliftr](https://upliftr.io) AI-native end-to-end suites.
Runs on the **public Upliftr image** (Chromium + engine baked in) — no install step.

## Usage

```yaml
# .github/workflows/upliftr.yml
name: Upliftr E2E
on: pull_request
jobs:
  e2e:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: upliftrhq/upliftr-action@v1
        with:
          suite: "suites/*.upliftr.yaml"
          file-issues: "github"          # optional; '' to disable
          anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
        env:
          GITHUB_TOKEN: ${{ github.token }}   # only for file-issues: github
```

Set **`ANTHROPIC_API_KEY`** as a repo secret. The exit code gates the PR
(`0` pass · `1` failed · `2` broken · `3` error).

| Input | Default | Purpose |
|---|---|---|
| `suite` | `suites/*.upliftr.yaml` | Path/glob of suites. |
| `file-issues` | `""` | Tracker to file failures into (e.g. `github`). |
| `anthropic-api-key` | *(required)* | The Anthropic key. |
| `out` | `upliftr-artifacts` | Report directory. |

Source & docs: <https://gitlab.com/upliftr/upliftr> · <https://docs.upliftr.io/guide/ci>
