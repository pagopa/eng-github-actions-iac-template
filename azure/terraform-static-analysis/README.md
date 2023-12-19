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
        uses: pagopa/eng-github-actions-iac-template/azure/terraform-static-analysis@feat/static-analysis-azure
        with:
          precommit_version: '1.83.5'
```
