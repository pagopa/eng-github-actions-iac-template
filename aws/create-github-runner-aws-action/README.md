# create-github-runner-aws-action

GitHub action for provisioning a dedicated GitHub self hosted runner in AWS as ECS task.

The runner will be labeled with a unique identifier associated. This identifier is also set as
tag of the ECS task. For using a runner provisioned like that, just set the requirement labels
of the self hosted runner as shown below.

## Usage example

This is just an example of a task that lists S3 buckets, running on a dedicated self hosted runner
provisioned by this action.

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
    environment: ${{ inputs.environment }}
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
    environment: dev
    needs: create_runner
    steps:
      - name: List S3 buckets
        shell: bash
        run: aws s3 ls
```

## Cleanup the runner

Use the [dedicated action](../remove-github-runner-aws-action/) for cleaning up a runner 
provisioned by this action.
