# Re-usable GitHub Actions

## `reusable-sync-file`

A GitHub Action to sync files between repositories.

This allows you to keep a copy of the file updated between multiple repositories.

Example usage:

```yaml
name: "Update OpenAPI specifications and raise PRs if changes needed"
permissions:
  contents: write
  # note that this also requires that GitHub Actions can raise PRs via steps in https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#preventing-github-actions-from-creating-or-approving-pull-requests
  pull-requests: write
on:
  schedule:
  - cron:  '30 5,17 * * *'
  workflow_dispatch: {}
jobs:
  sync-petstore:
    uses: jamietanna/actions/.github/workflows/reusable-sync-file.yml@main
    with:
      identifier: oapi-codegen-petstore
      remote_slug: deepmap/oapi-codegen
      remote_path: examples/petstore-expanded/petstore-expanded.yaml
      local_path: petstore.yaml
    secrets:
      # as this is a public repo, we can sync it easily with the inbuilt token
      cross_repo_github_token: ${{ secrets.GITHUB_TOKEN }}

  # and for a private repo that has two specs in it
  sync-private-admin-api:
    uses: jamietanna/actions/.github/workflows/reusable-sync-file.yml@main
    with:
      identifier: private-admin-api
      remote_slug: jamietanna/example-github-actions-sync-files-private
      remote_path: api/admin-api.yml
      local_path: admin-api.yml
    secrets:
      # this requires a PAT
      cross_repo_github_token: ${{ secrets.PAT }}

  sync-private-another-api:
    uses: jamietanna/actions/.github/workflows/reusable-sync-file.yml@main
    with:
      identifier: private-another-api
      remote_slug: jamietanna/example-github-actions-sync-files-private
      remote_path: api/another-api.yml
      local_path: another-api.yml
    secrets:
      # this requires a PAT
      cross_repo_github_token: ${{ secrets.PAT }}
```
