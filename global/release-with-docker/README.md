# Release with docker action

This action allows to generate a release + tags in github
and use `https://github.com/cycjimmy/semantic-release-action/commits/main` action

and build and push a docker image with the new release version

## How to use

### Pre requisites

see `https://github.com/pagopa/eng-github-actions-iac-template/blob/72b6f5aed95ad11b096f8c17e6d66d5cc0f1dd90/global/release-action/README.md`

### Action

Create inside the folder `.github/workflow` a yaml file called `release.yaml` with this content

```yaml
name: Release

on:
  # Trigger the workflow on push or pull request,
  # but only for the main branch
  push:
    branches:
      - main
    paths-ignore:
      - "CODEOWNERS"
      - "**.md"
      - ".**"

jobs:
  release:
    name: Release
    runs-on: ubuntu-latest
    steps:
      - name: Release with docker action
        id: release
        uses: pagopa/eng-github-actions-iac-template/global/release-with-docker@main #
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
```
