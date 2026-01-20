# PAT作成

Expiration: No expiration

Repository access: Only select repositories

Permissions
- Contents: Read and write
- Metadata: Read-only
- Pull requests: Read and write
- Workflows: Read and write

# Repository Settings

## Secrets and variables

### Actions

Repository secretsに先ほどの値を登録(key:`MY_RELEASE_PLEASE_TOKEN`)

## Rules > Rulesets

Add branch ruleset

### Ruleset name: protect main

Bypass list
- Repository admin

Target branches: main

Rules
- Restrict updates
- Restrict deletions
- Require linear history
- Block force pushes

### Ruleset: develop branch
- Bypass list: empty
- Target branch: develop
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
