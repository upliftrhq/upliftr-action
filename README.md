# Upliftr for GitHub Actions

Gate your pull requests with [Upliftr](https://upliftr.io): AI agents drive your
app in a real browser, run your suites, and when a flow breaks they trace it to
the backend root cause. Runs on the **public Upliftr image** (Chromium and the
engine baked in), so there is no install step.

## Usage

```yaml
# .github/workflows/upliftr.yml
name: Upliftr
on: pull_request
jobs:
  upliftr:
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
(`0` pass, `1` failed, `2` broken, `3` error).

| Input | Default | Purpose |
|---|---|---|
| `suite` | `suites/*.upliftr.yaml` | Path/glob of suites to run. |
| `file-issues` | `""` | Tracker to file failures into (e.g. `github`). |
| `anthropic-api-key` | *(required)* | The Anthropic key the agents run on. |
| `out` | `upliftr-artifacts` | Report directory. |

Source and docs: <https://github.com/upliftrhq/upliftr> and <https://docs.upliftr.io/guide/ci>
