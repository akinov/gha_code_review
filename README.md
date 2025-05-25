# AI Code Review GitHub Action

AIを活用したコードレビューを自動化するGitHub Actionsワークフローです。Pull Requestが作成・更新されるたびに、AI（GitHub Models API）がコードの差分を自動的にレビューし、コメントを投稿します。

## 🚀 機能

- **自動コードレビュー**: Pull Request作成時にAIが自動的にコードをレビュー
- **包括的な評価**: コード品質、セキュリティ、パフォーマンス、ベストプラクティスを評価
- **詳細なフィードバック**: 改善提案と具体的な修正方法を提供
- **日本語対応**: 日本語でのレビューコメントを生成
- **デバッグ機能**: 詳細なログ出力でトラブルシューティングをサポート

## 📋 評価観点

AIレビューでは以下の観点でコードを評価します：

1. **コードの品質・可読性**
2. **セキュリティ上の問題**
3. **パフォーマンスへの影響**
4. **バグの可能性**
5. **ベストプラクティスの遵守**

## 🛠️ セットアップ

### 前提条件

- GitHub Repositoryでの作業
- GitHub Models API（ベータ版）へのアクセス権限
- Pull Requestでの動作確認

### インストール

1. このリポジトリをフォークまたはクローン
2. `.github/workflows/review.yml`を自分のリポジトリにコピー
3. 必要に応じて設定をカスタマイズ

### 必要な権限設定

ワークフローが正常に動作するために、以下の権限が必要です：

```yaml
permissions:
  pull-requests: write  # PRにコメントを投稿するため
  contents: read        # リポジトリの内容を読み取るため
  models: read          # GitHub Models APIを使用するため
```

## ⚙️ 設定

### 環境変数

ワークフローでは以下の環境変数を設定できます：

| 変数名 | デフォルト値 | 説明 |
|--------|-------------|------|
| `AI_MODEL` | `openai/gpt-4.1` | 使用するAIモデル |
| `LIST_MODELS` | `false` | 利用可能なモデル一覧を表示するかどうか |
| `DIFF_CHAR_LIMIT` | `20000` | 差分テキストの文字数制限 |

### AIモデルの変更

使用するAIモデルを変更する場合は、`.github/workflows/review.yml`の`env`セクションを編集してください：

```yaml
env:
  AI_MODEL: "openai/gpt-4.1"  # 他のモデルに変更可能
  LIST_MODELS: "false"
  DIFF_CHAR_LIMIT: "20000"    # 差分テキストの文字数制限
```

利用可能なモデルを確認するには、`LIST_MODELS`を`"true"`に設定してワークフローを実行してください。

### 差分文字数制限の変更

大きなPull Requestを処理する場合は、`DIFF_CHAR_LIMIT`を調整できます：

```yaml
env:
  DIFF_CHAR_LIMIT: "30000"  # より大きな差分を処理する場合
```

**注意**: 文字数制限を大きくしすぎると、AIモデルのトークン制限に達する可能性があります。

## 🔧 カスタマイズ

### レビュープロンプトの変更

レビューの内容をカスタマイズするには、`.github/workflows/review.yml`の`REVIEW_PROMPT`環境変数を編集してください：

```yaml
env:
  REVIEW_PROMPT: |
    カスタムプロンプトをここに記述
    レビューの観点や形式を指定できます
```

### トリガー条件の変更

ワークフローのトリガー条件を変更するには、`on`セクションを編集してください：

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened]
    # branches: [main]  # 特定のブランチのみに限定する場合
```

## 📖 使用方法

1. Pull Requestを作成または更新
2. 自動的にワークフローが開始
3. AIがコードの差分を分析
4. レビューコメントがPull Requestに投稿される

## 🐛 トラブルシューティング

### よくある問題

#### 1. ワークフローが実行されない
- リポジトリの設定でGitHub Actionsが有効になっているか確認
- ワークフローファイルの権限設定が正しいか確認

#### 2. AIレビューが生成されない
- GitHub Models APIへのアクセス権限があるか確認
- 指定したAIモデルが利用可能か確認（`LIST_MODELS: "true"`で確認）

#### 3. コメントが投稿されない
- `pull-requests: write`権限が設定されているか確認
- `GITHUB_TOKEN`が正しく設定されているか確認

### デバッグモード

ワークフローには詳細なデバッグログが含まれています。問題が発生した場合は、GitHub ActionsのログでDEBUGメッセージを確認してください。

## 🔒 セキュリティ

- `GITHUB_TOKEN`は自動的に提供されるため、追加のシークレット設定は不要
- GitHub Models APIはGitHubの認証システムを使用
- レビュー対象のコードは外部サービスに送信されるため、機密情報の取り扱いに注意

## 📄 ライセンス

MIT License

## 🤝 コントリビューション

Issue、Pull Requestを歓迎します。

## 📚 参考資料

- [GitHub Models API](https://docs.github.com/en/rest/models)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Actions Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)

---

**注意**: このワークフローはGitHub Models API（ベータ版）を使用しており、APIの仕様や利用可能性が変更される可能性があります。
