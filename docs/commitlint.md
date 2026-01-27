# Commitlint

GitHub Actions ワークフローで Commitlint が動作しているため、非 Node.js 環境でもコミットメッセージのチェックが行われます。

## ワークフローでの設定

GitHub Actions ワークフロー（`.github/workflows/commitlint.yml`）では、以下の設定で Commitlint が動作しています：

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  defaultIgnores: false
};
```

### 設定の説明

- **`extends: ['@commitlint/config-conventional']`**: Conventional Commits 形式のルールを適用します
- **`defaultIgnores: false`**: デフォルトで無視されるコミット（マージコミットなど）も含めて、すべてのコミットメッセージを検証します。これにより、Conventional Commits 形式以外のコミットメッセージを自動的に弾くことができます。GitFlow を採用しており、基本的に線形コミットにしかならない前提のため、この設定にしています

ワークフローでは、PR 内のすべてのコミット（`--from` から `--to` まで）が検証されます。

## Node.js プロジェクトの場合

Node.js プロジェクトの場合は、設定ファイル（`commitlint.config.cjs`）をリポジトリに含めることで、ローカルでもコミットメッセージの検証ができます。

### 設定ファイルの作成

リポジトリのルートに `commitlint.config.cjs` を作成します。

PR検証では fixup などを禁止していますが、ローカル環境ではそこまで厳しくしなくてよいため、`defaultIgnores` を除いた標準的な設定でも問題ありません：

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```

ワークフローと同じ厳格な設定にしたい場合は、`defaultIgnores: false` を追加してください：

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  defaultIgnores: false
};
```

### インストール

```sh
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

### ワークフローの更新

リポジトリに設定ファイルが含まれている場合、commitlint は自動的にその設定ファイルを使用します。ローカルと GitHub Workflow でルールを別にしないのであれば、ワークフロー（`.github/workflows/commitlint.yml`）から設定ファイル生成部分を削除できます：

```yaml
- name: Install commitlint
  run: |
    npm install --save-dev @commitlint/config-conventional @commitlint/cli
    # 設定ファイル生成部分は削除（リポジトリに含まれる設定ファイルが自動的に使用される）
```

リポジトリに設定ファイルが存在する場合、npm install の後に生成した設定ファイルよりもリポジトリの設定ファイルが優先されます。

## 参考資料

- [@commitlint/config-conventional](https://www.npmjs.com/package/@commitlint/config-conventional)
