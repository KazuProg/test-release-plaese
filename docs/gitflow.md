# GitFlow

## 概要

GitFlowは、機能開発、リリース、ホットフィックスの管理を明確に分離し、安定した本番環境を維持しながら並行開発を可能にするブランチ戦略です。

## ブランチ戦略

### 主要ブランチ

#### `main` ブランチ
- **役割**: 本番環境にリリースされている状態を常に反映するブランチ
- **特徴**:
  - 常にデプロイ可能な状態を保つ
  - `develop`ブランチからのみMergeされる
  - 各コミットはタグ付けされ、リリースバージョンとして管理される
- **保護設定**: ブランチ保護が有効で、直接のコミットは禁止

#### `develop` ブランチ
- **役割**: 次回リリースに向けた開発統合ブランチ
- **特徴**:
  - `feature/*`ブランチからMergeされる
  - リリース準備が整った機能を統合する
  - 継続的な統合テストが実行される
- **保護設定**: ブランチ保護が有効で、直接のコミットは禁止

### 機能開発ブランチ

#### `feature/*` ブランチ
- **役割**: 新機能開発用のブランチ
- **命名規則**: `feature/機能名` または `feature/ISSUE番号-機能名`
- **作成元**: `develop`ブランチから分岐
- **マージ先**: 開発完了後、`develop`ブランチにマージ
- **例**:
  - `feature/user-authentication`
  - `feature/123-add-login-form`

### 緊急修正ブランチ

#### `hotfix/*` ブランチ
- **役割**: 本番環境の緊急バグ修正用のブランチ
- **命名規則**: `hotfix/修正内容` または `hotfix/ISSUE番号-修正内容`
- **作成元**: `main`ブランチから分岐
- **マージ先**: 修正完了後、`main`と`develop`の両方にマージ

## ワークフロー

### 1. 機能開発の流れ

```
develop → feature/new-feature → develop → main
```

1. `develop`ブランチから`feature/*`ブランチを作成
2. 機能開発を実施
3. 完了後、`develop`ブランチに対してPull Requestを作成
4. コードレビューと承認後、`develop`にマージ
5. `develop`が十分に安定したら、`main`にマージ

### 2. リリースフロー

```
develop → main
```

- `develop`ブランチの内容がリリース準備が整ったら、`main`ブランチにマージ
- マージ時にリリースタグが作成される

## ブランチ保護

`main`と`develop`ブランチにはブランチ保護が設定されており、直接のコミットは禁止されています。すべての変更はPull Request経由で行います。

## 注意点・ポイント

- `feature/*`ブランチは1つの機能単位で作成する
- ブランチ名は明確で分かりやすいものにする
- 開発完了後は速やかにマージし、長期間放置しない
- `main`や`develop`に直接コミットしない
- 古い`feature/*`ブランチを残さない

## 本リポジトリでの構成

### リリースプロセス

1. `feature/*`ブランチから`develop`へのPull Requestがマージされたことをトリガーに、`release-please.yml`ワークフローが実行される
2. release-please-actionが`develop`の変更を検知し、CHANGELOGをまとめた`develop`へのマージ用のPull Requestを自動的に発行する
3. release-pleaseのPull Requestを`develop`にマージすると、リリースタグ（`v*`形式）が作成される
4. タグの作成（push）をトリガーに、`sync-main.yml`ワークフローが実行される
5. `sync-main.yml`が`main`ブランチに`develop`ブランチをfast-forwardマージする

これにより、`develop`から`main`へのマージが自動化されている。

### ブランチ保護設定

`main`と`develop`ブランチは厳しくブランチ保護されており、以下の制限が設定されています：

- 直接のコミット・更新・削除は禁止
- 強制プッシュは禁止
- 線形履歴が必須
- `develop`ブランチはPull Request必須（Rebaseマージのみ）、ステータスチェック必須

詳細な設定については、[GitHub設定ドキュメント](./github-settings.md)を参照してください。

### hotfixブランチについて

現在、`hotfix/*`ブランチは使用せず、`feature/*`ブランチとして対応している。

## 参考資料

- [GitFlow公式ドキュメント](https://nvie.com/posts/a-successful-git-branching-model/)
