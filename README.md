# gh-workflows

Reusable GitHub Actions workflows shared across non7top's repos.

## release-please.yml

Runs `release-please` on every push to the calling repo's default branch:
maintains the running release PR, keeps the `RELEASE` label colored by
predicted bump type (patch/minor/major), and tags + publishes a GitHub
release once that PR merges.

Caller repo needs `release-please-config.json` and
`.release-please-manifest.json` at its root (paths are overridable via
inputs if you name them differently).

```yaml
# .github/workflows/release-please.yml
name: release-please

on:
  push:
    branches: [main]

permissions:
  contents: write
  pull-requests: write

jobs:
  release-please:
    uses: non7top/gh-workflows/.github/workflows/release-please.yml@main
    permissions:
      contents: write
      pull-requests: write
```

## release-please-preview.yml

Runs on every PR against the default branch and forecasts (via
[`non7top/release-please-forecast`](https://github.com/non7top/release-please-forecast))
what release-please would do if this PR were the next thing merged, coloring
the same `RELEASE` label to match ahead of merge.

```yaml
# .github/workflows/release-please-preview.yml
name: release-please-preview

on:
  pull_request:
    types: [opened, synchronize, reopened, edited]
    branches: [main]

permissions:
  contents: read
  pull-requests: write

jobs:
  preview:
    uses: non7top/gh-workflows/.github/workflows/release-please-preview.yml@main
    permissions:
      contents: read
      pull-requests: write
```

## Versioning

Callers should pin to `@main` for now (single-maintainer repo, no tagged
releases yet) and re-check after pulling in changes, since a breaking change
here isn't gated by semver the way a tagged action would be.
