# ブランチ保護（main/developへの直接コミット防止）

本リポジトリでは[GitFlow](./gitflow.md)を採用しており、`main`ブランチと`develop`ブランチは保護されたブランチです。

- **`main`ブランチ**: 本番環境にリリースされている状態を常に反映するブランチ。直接のコミットは禁止されており、`develop`ブランチからのみMergeされます。
- **`develop`ブランチ**: 次回リリースに向けた開発統合ブランチ。`feature/*`ブランチからMergeされ、直接のコミットは禁止されています。

GitHubでは[ブランチ保護ルール](./github-settings.md)を設定していますが、それに加えてローカルのGit hooksでも設定することで、誤って保護されたブランチにコミットしようとした際に早期に検知できます。

## Git hooksでの設定

`.git/hooks/pre-commit` または `.git/hooks/pre-push` に以下のスクリプトを配置します：

```sh
#!/bin/sh

branch="$(git rev-parse --abbrev-ref HEAD)"

if [ "$branch" = "main" ] || [ "$branch" = "develop" ]; then
  echo "Error: Forbidden to commit to $branch branch."
  exit 1
fi
```

スクリプトに実行権限を付与する必要があります：

```sh
chmod +x .git/hooks/pre-commit
# または
chmod +x .git/hooks/pre-push
```

## Node.jsプロジェクトの場合

Node.jsプロジェクトでは、[husky](https://typicode.github.io/husky/)を使用してGit hooksを管理することもできます。

### インストール

```sh
npm install --save-dev husky
npx husky init
```

### 設定

`.husky/pre-commit` または `.husky/pre-push` に以下のスクリプトを配置します：

```sh
#!/bin/sh

branch="$(git rev-parse --abbrev-ref HEAD)"

if [ "$branch" = "main" ] || [ "$branch" = "develop" ]; then
  echo "Error: Forbidden to commit to $branch branch."
  exit 1
fi
```
