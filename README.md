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

# husky

```
npm install --save-dev husky
npx husky init
```
