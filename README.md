# HighLights

1. The release job resolves its notes from `CHANGELOG.md`.
2. The publish job supports syncing pkgs to cnpm, including monorepo.

# Template

```yml
name: Release and Publish

on:
  push:
    tags:
      - v*

jobs:
  release:
    permissions:
      contents: write
      id-token: write
    uses: lvjiaxuan/github-action-templates/.github/workflows/lvr-release.yml@main
    secrets: inherit

  publish:
    uses: lvjiaxuan/github-action-templates/.github/workflows/lvr-publish.yml@main
    with:
      sync_cnpm: true
    secrets: inherit
```