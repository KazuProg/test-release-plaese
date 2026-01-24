# Commitlint

GitHub Actions ワークフローで Commitlint が動作しているため、非 Node.js 環境でもコミットメッセージのチェックが行われます。

## Node.js プロジェクトの場合

Node.js プロジェクトの場合は、以下のように Commitlint をインストールしてローカルでも動作させることができます。

```sh
npm install --save-dev @commitlint/config-conventional @commitlint/cli
echo "export default {extends: ['@commitlint/config-conventional']};" > commitlint.config.js
```

## 参考資料

- [@commitlint/config-conventional](https://www.npmjs.com/package/@commitlint/config-conventional)
