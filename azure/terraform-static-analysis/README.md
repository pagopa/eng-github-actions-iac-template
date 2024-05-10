# Terraform azure static analysis

This action scans the repository, identifying all folders containing Terraform code and triggers a pre-commit step.

Note: The project must have [this structure](https://github.com/pagopa/terraform-infrastructure-template).

The action:

- calls the `Init terraform folders` to inizialize projects
- runs pre-commit step

## Usage

``` yaml
name: Static Analysis PR

on:
  push:
    branches-ignore:
      - main

jobs:
  static_analysis:
    runs-on: ubuntu-22.04

    steps:
      - name: Static Analysis
        uses: pagopa/eng-github-actions-iac-template/azure/terraform-static-analysis@v1.7.0
        with:
          precommit_version: 'v1.89.1@sha256:c9646748faf2914cbac342e9c06a7b5865d06d2f'
```
