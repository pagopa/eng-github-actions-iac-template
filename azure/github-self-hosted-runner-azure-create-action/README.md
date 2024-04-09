# github-self-hosted-runned-azure-create-action

## How to use

```yaml
jobs:
  create_runner:
    name: Create Runner
    runs-on: ubuntu-22.04
    environment:
      name: ${{ inputs.environment }}
    outputs:
      runner_name: ${{ steps.create_github_runner.outputs.runner_name }}
    steps:
      - name: Create GitHub Runner
        id: create_github_runner
        # from https://github.com/pagopa/eng-github-actions-iac-template/tree/main/azure/github-self-hosted-runner-azure-create-action
        uses: pagopa/eng-github-actions-iac-template/azure/github-self-hosted-runner-azure-create-action@main
        with:
          client_id: ${{ secrets.CD_CLIENT_ID }}
          tenant_id: ${{ secrets.TENANT_ID }}
          subscription_id: ${{ secrets.SUBSCRIPTION_ID }}
          container_app_environment_name: ${{ vars.CONTAINER_APP_ENVIRONMENT_NAME }}
          resource_group_name: ${{ vars.CONTAINER_APP_ENVIRONMENT_RESOURCE_GROUP_NAME }} # RG of the runner
          pat_token: ${{ secrets.BOT_TOKEN_GITHUB }}
```
