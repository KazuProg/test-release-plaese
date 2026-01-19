# Git Flow 検証など

## ブランチ運用

- `main`
  - リリースされている状態のブランチ
  - `develop`からのみMergeされる(GitHub Workflowで自動化)
- `develop`: リリースを控えているブランチ
  - `feature/*`ブランチからMergeされる
- `feature/*` 機能開発ブランチ

`main`,`develop`はブランチ保護をし、勝手にCommitなどされないようにしている。
リポジトリオーナーはその制限を突破できるので、huskyでも縛っている

## PR運用

Rebase Merge
- コミットログがConventionalCommits形式で統一される

## Commitメッセージ

Conventional Commits

commitlintを使用してコミット時に自動検証
