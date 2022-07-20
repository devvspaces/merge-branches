# Merge branches
A Github action to merge one branch to the other

## Usage Example
Simple example to automatically merge master branch to production branch when new update is pushed to the master branch

```yml
name: Update Production

on:
  push:
    branches:
      - master

permissions:
  # Need `contents: read` to checkout the repository
  # Need `contents: write` to merge branches
  contents: write

jobs:
  update-production:
    name: Merge master into production after a PR is merged
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 0 # Let's get all the branches
      - name: Merge Master to Production branch
        uses: devvspaces/merge-branches@v1
        with:
          token: ${{ github.token }}
          from_branch: master
          to_branch: production

```

## Required Usage
This shows statements that are needed to run the action

```yml
permissions:
  # [Learn more about automatic token authentication permissions]
  # (https://docs.github.com/en/actions/security-guides/automatic-token-authentication#permissions-for-the-github_token)
  # Permissions required for merging branches
  # Need `contents: read` to checkout the repository
  # Need `contents: write` to merge branches
  contents: write
```

```yml
- uses: devvspaces/merge-branches@v1
  with:
    # Personal access token (PAT) used to merge branches. The PAT is configured
    # with the local git config, which enables your scripts to run authenticated git
    # commands.
    #
    # We recommend using a service account with the least permissions necessary. Also
    # when generating a new PAT, select the least scopes necessary.
    #
    # [Learn more about creating and using encrypted secrets]
    # (https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)
    token: ${{ github.token }}
    
    # Branch to merge to other branch
    from_branch: master
    
    # Branch to merge other branch on
    to_branch: production
```
