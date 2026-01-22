# Generate PAT

## Account settings > Developer settings

### Personal access Tokens > Fine-grained tokens
Generate new token
- Token name: `test-for-workflow`
- Expiration: No expiration
- Repository access: Only select repositories
    - (your repository)
- Permissions > Repositories
    - Contents: Read and write
    - Metadata: Read-only
    - Pull requests: Read and write
    - Workflows: Read and write

# Repository Settings

## Rules > Rulesets
New ruleset > New branch ruleset

### Ruleset: main branch
- Bypass list
    - Repository admin
- Target branches
    - main
- Rules
    - Restrict creations
    - Restrict updates
    - Restrict deletions
    - Require linear history
    - Block force pushes

### Ruleset: develop branch
- Bypass list
    - (empty)
- Target branch
    - develop
- Rules
    - Restrict creations
    - Restrict deletions
    - Require linear history
    - Require a pull request before merging
        - Required approvals: 0
        - Dismiss stale pull request approvals when new commits are pushed
        - Require conversation resolution before merging
        - Allowed merge methods: Rebase
    - Require status checks to pass
        - Do not require status checks on creation
        - commitlint
        - (other ci checks)
    - Block force pushes

## Secrets and variables > Actions
New repository secret
- Name: `MY_RELEASE_PLEASE_TOKEN`
- Secret: `{Your PAT}`
