# GitHub Actions デプロイ設定ガイド

## 概要

このリポジトリ（`hugo-github-pages`）でHugoサイトをビルドし、生成された静的ファイルを`souseiji.github.io`リポジトリに自動デプロイします。

## セットアップ手順

### 1. Personal Access Token (PAT) の作成

GitHub Actionsが`souseiji.github.io`リポジトリにプッシュできるようにするため、Personal Access Tokenが必要です。

1. GitHubにログインし、[Settings > Developer settings > Personal access tokens > Tokens (classic)](https://github.com/settings/tokens) にアクセス
2. **Generate new token** → **Generate new token (classic)** をクリック
3. 以下の設定を行う：
   - **Note**: `Hugo Deploy to GitHub Pages` など、わかりやすい名前
   - **Expiration**: `No expiration` または適切な期限
   - **Select scopes**: 
     - ✅ `repo` (Full control of private repositories) にチェック
4. **Generate token** をクリック
5. 生成されたトークンをコピー（**この画面を閉じると二度と表示されません**）

### 2. Secretの設定

1. このリポジトリ（`hugo-github-pages`）のページに移動
2. **Settings** → **Secrets and variables** → **Actions** をクリック
3. **New repository secret** をクリック
4. 以下を入力：
   - **Name**: `PERSONAL_ACCESS_TOKEN`
   - **Secret**: 先ほどコピーしたトークンを貼り付け
5. **Add secret** をクリック

### 3. デプロイ先リポジトリの設定確認

`souseiji.github.io`リポジトリで以下を確認：

1. **Settings** → **Pages** にアクセス
2. **Source** が `Deploy from a branch` になっていることを確認
3. **Branch** が `main` / `/ (root)` になっていることを確認

### 4. 動作確認

1. このリポジトリ（`hugo-github-pages`）に変更をコミット＆プッシュ：
   ```bash
   git add .
   git commit -m "feat: GitHub Actionsデプロイ設定を追加"
   git push origin main
   ```

2. **Actions** タブでワークフローの実行状況を確認

3. 成功したら、`https://souseiji.github.io` でサイトが表示されることを確認

## ワークフローの動作

### トリガー
- `main`ブランチへのプッシュ時に自動実行
- 手動実行も可能（Actionsタブから）

### 処理フロー
1. ソースコードをチェックアウト（サブモジュール含む）
2. Hugo（最新版、extended版）をセットアップ
3. `hugo --minify`でサイトをビルド
4. `public/`フォルダの内容を`souseiji.github.io`リポジトリの`main`ブランチにプッシュ

## トラブルシューティング

### ワークフローが失敗する場合

**エラー: `remote: Permission to souseiji/souseiji.github.io.git denied`**
- Personal Access Tokenが正しく設定されていない
- トークンに`repo`スコープが含まれていない
- Secretの名前が`PERSONAL_ACCESS_TOKEN`になっていない

**エラー: `Error: Unable to locate executable file: hugo`**
- Hugo setup stepが失敗している
- ワークフローファイルの構文を確認

**サイトが表示されない**
- `souseiji.github.io`リポジトリのGitHub Pages設定を確認
- デプロイが完了してから数分待つ
- ブラウザのキャッシュをクリア

### カスタムドメインを使用する場合

`deploy.yml`の最後の部分を以下のように変更：

```yaml
          # CNAMEファイルを保持（カスタムドメインを使用する場合）
          cname: your-domain.com
```

## 参考リンク

- [Hugo Documentation](https://gohugo.io/documentation/)
- [GitHub Pages Documentation](https://docs.github.com/pages)
- [peaceiris/actions-hugo](https://github.com/peaceiris/actions-hugo)
- [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)

## 🔒 セキュリティに関する注意事項

### Personal Access Tokenの管理

**重要**: Personal Access Token (PAT) は非常に強力な認証情報です。以下の点に注意してください：

1. **トークンの保護**
   - トークンは絶対にコードやコミットメッセージに含めないでください
   - GitHub Secretsを使用して安全に管理してください
   - トークンを共有したり、公開したりしないでください

2. **トークンの権限**
   - 必要最小限の権限（`repo`スコープのみ）を付与してください
   - 定期的にトークンの使用状況を確認してください

3. **トークンの有効期限**
   - セキュリティのため、有効期限を設定することを推奨します
   - 期限切れ前に新しいトークンを生成し、Secretsを更新してください

4. **トークンの漏洩時の対応**
   - トークンが漏洩した可能性がある場合は、直ちに無効化してください
   - [GitHub Settings > Developer settings > Personal access tokens](https://github.com/settings/tokens) から削除できます
   - 新しいトークンを生成し、Secretsを更新してください

### リポジトリのセキュリティ

1. **公開リポジトリの注意点**
   - このリポジトリは公開されているため、機密情報を含めないでください
   - APIキー、パスワード、個人情報などは絶対にコミットしないでください

2. **`.gitignore`の確認**
   - 環境変数ファイル（`.env`など）が除外されていることを確認してください
   - 機密情報を含む可能性のあるファイルは必ず除外してください

3. **コミット履歴の確認**
   - 過去のコミットに機密情報が含まれていないか定期的に確認してください
   - 万が一含まれている場合は、[BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)などのツールで削除してください

