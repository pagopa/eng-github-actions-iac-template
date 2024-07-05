# Docker build and push

Docker build and push an image into github container registry

## How to use

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
