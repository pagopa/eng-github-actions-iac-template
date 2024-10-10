# 📁 Get Modified Directory and Sparse Checkout

This GitHub Actions workflow retrieves all modified directories and performs a sparse checkout. It is useful for situations where you want to download only specific parts of a repository based on recent changes.

## Description

The workflow:
1. Checks out the repository.
2. Retrieves modified paths based on include/exclude criteria.
3. Determines if a static analysis should be run and sets the `run_static_analysis` environment variable.
4. Downloads only the modified folders using git's sparse checkout functionality.

## Inputs

- `starting_index` (required): Starting index for path filtering. Default: `'/'`.
- `default_end_index` (required): Default end index for path slicing. Default: `2`.
- `include_patterns` (required): Patterns to include in path filtering. Default:
  ```yaml
  "src,domains"
  ```
- `exclude_ignored` (required): Whether to exclude ignored paths. Default: `True`.

## Outputs

- `modified_paths`: List of modified paths obtained.
- `run_static_analysis`: Determines if static analysis should run.

## Example Usage

```yaml
      - name: 🔨 Get Modified Paths
        id: get-paths
        uses: pagopa/eng-github-actions-iac-template/global/get-modifed-folders@main
        with:
          starting_index: "src"
          default_end_index: 3
          include_patterns: "src,domains"
```

## Notes

- The file uses Python to perform the main operations of filtering and determining modified paths.
- If the modified paths match the patterns defined in `include_patterns`, the workflow will execute a static analysis and download only the modified directories.
