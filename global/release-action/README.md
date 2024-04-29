# Release action

This action allows to generate a release + tags in github

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
      - name: Release action
        uses: pagopa/eng-github-actions-iac-template/global/release-action@release-action # 
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
```

and copy the file `.releaserc.json` into the project root folder to configure the plugins
to use the release action pipeline
