# Workflow Templates

1. [lvr-release](.github/workflows/lvr-release.yml)
2. [lvr-publish](.github/workflows/lvr-publish.yml)(to npm)
3. [update-deps](.github/workflows/update-deps.yml)

# Usage

## Release and Publish are usually used together.

Based on my own [release tool](github.com/lvjiaxuan/release).

features:
1. The release workflow extracts release's notes from `CHANGELOG.md` which is created by my tool.
2. The publish workflow supports syncing published pkgages to cnpm.

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
    uses: lvjiaxuan/github-action-templates/.github/workflows/lvr-release.yml@main
    secrets: inherit

  publish:
    uses: lvjiaxuan/github-action-templates/.github/workflows/lvr-publish.yml@main
    with:
      sync_cnpm: true
    secrets: inherit
```

## Update dependencies

### Ways to set the user's email, which is a required param.

1. A hard code on inputs. Recommend using GitHub's [noreply email](https://github.com/settings/emails).
2. If `inputs.email` isn't provided, the `vars.ACTOR_EMAIL` will be read. Refer to actions secrets and variables settings :point_right: https://github.com/{actor}/{repo}/settings/secrets/actions .

> [!TIP]
> :point_right: [cron syntax help](https://crontab.guru/examples.html)

```yml
name: Update Dependencies

permissions:
  pull-requests: write
  contents: write

on:
  workflow_dispatch: {}
  schedule:
    - cron: 0 0 * * SAT

jobs:
  update-deps:
    uses: lvjiaxuan/github-action-templates/.github/workflows/update-deps.yml@main
    with:
      email: xxx@xx.xx # Explicitly set `inputs.email`, if `vars.ACTOR_EMAIL` isn't set on repo.
```

[more inputs](https://github.com/lvjiaxuan/github-action-templates/blob/main/.github/workflows/update-deps.yml)

# Other recommended actions

1. [update-repository-description](https://github.com/zhengbangbo/update-repository-description) by @zhengbangbo
