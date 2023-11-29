# remove-github-runner-aws-action

GitHub action for removing a GitHub self hosted runner in AWS as ECS task.
Removing means: remove it from GitHub and stop the task in AWS.

It is indended to be used for cleaning up runners created with the [dedicated action](../create-github-runner-aws-action/).

The removal works by looking up runner and task by label and tag respectively, both with their 
unique identifier generated in creation phase.

## Usage example

This is just an example of a task that lists S3 buckets, running on a dedicated self hosted runner
provisioned by the create action, that will be cleaned up by this action.

```yml
name: Simple example

permissions:
  contents: read
  id-token: write

jobs:
  create_runner:
    name: Create Self-Hosted Runner
    runs-on: ubuntu-22.04
    strategy:
      matrix:
        index: [1]
      fail-fast: true
    outputs:
      runner_name: ${{ steps.start_runner.outputs.runner_name }}

    steps:
      - name: Start GitHub Runner
        id: start_runner
        uses: pagopa/eng-github-actions-iac-template/create-github-runner-aws-action@v1
        with:
          aws_region: ${{ vars.AWS_REGION }}
          iam_role_arn: ${{ vars.IAM_ROLE }}
          ecs_cluster_name: ${{ vars.ECS_CLUSTER_NAME }}
          ecs_task_definition: ${{ vars.ECS_TASK_DEFINITION }}
          ecs_container_name: github-runner
          ecs_task_subnet_id: ${{ vars.SUBNET_ID }}
          ecs_task_sec_group: ${{ vars.SEC_GROUP_ID }}
          pat_token: ${{ secrets.BOT_TOKEN }}

  list_s3_buckets:
    name: Just an example, list S3 buckets
    # set here the required label of the hosted runner as the runner name returned by the create job
    runs-on: [self-hosted, "${{ needs.create_runner.outputs.runner_name }}"]
    needs: create_runner
    steps:
      - name: List S3 buckets
        shell: bash
        run: aws s3 ls

  delete_runner:
    name: Delete Self-Hosted Runner
    needs: [create_runner, list_s3_buckets]
    strategy:
      matrix:
        index: [1]
      fail-fast: true
    if: ${{ always() }}
    runs-on: ubuntu-22.04

    steps:
      - name: Remove Github Runner
        id: remove_runner
        uses: pagopa/eng-github-actions-iac-template/remove-github-runner-aws-action@v1
        with:
          aws_region: ${{ vars.AWS_REGION }}
          iam_role_arn: ${{ vars.IAM_ROLE }}
          ecs_cluster_name: ${{ vars.ECS_CLUSTER_NAME }}
          pat_token: ${{ secrets.BOT_TOKEN }}
          runner_name: ${{ needs.create_runner.outputs.runner_name }}
```
