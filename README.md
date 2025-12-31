# 河添ひろし公式サイト - Hugoソースリポジトリ

このリポジトリは、[河添ひろし公式サイト](https://souseiji.github.io)のHugoソースコードを管理しています。

## 🚀 自動デプロイ

`main`ブランチにプッシュすると、GitHub Actionsが自動的に：
1. Hugoサイトをビルド
2. 生成されたファイルを[souseiji.github.io](https://github.com/souseiji/souseiji.github.io)リポジトリにデプロイ

## 📁 リポジトリ構成

```
hugo-github-pages/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actionsワークフロー
├── archetypes/                 # コンテンツテンプレート
├── content/                    # サイトコンテンツ（記事など）
├── static/                     # 静的ファイル（画像など）
├── themes/                     # Hugoテーマ（LoveIt）
├── hugo.toml                   # サイト設定
├── .gitignore                  # Git除外設定
└── DEPLOY_SETUP.md            # デプロイ設定ガイド
```

## 🛠️ ローカル開発

### 前提条件
- [Hugo Extended版](https://gohugo.io/installation/)がインストールされていること
- Git

### セットアップ

1. リポジトリをクローン：
```bash
git clone --recurse-submodules https://github.com/souseiji/hugo-github-pages.git
cd hugo-github-pages
```

2. 開発サーバーを起動：
```bash
hugo server -D
```

3. ブラウザで `http://localhost:1313` にアクセス

### 新規記事の作成

```bash
hugo new posts/my-new-post.md
```

## 📝 コンテンツの更新方法

1. `content/`ディレクトリ内のMarkdownファイルを編集
2. 変更をコミット＆プッシュ：
```bash
git add .
git commit -m "記事を追加: タイトル"
git push origin main
```
3. GitHub Actionsが自動的にビルド＆デプロイ

## ⚙️ 初回セットアップ

GitHub Actionsでのデプロイを有効にするには、`DEPLOY_SETUP.md`を参照してください。

主な手順：
1. Personal Access Token (PAT) を作成
2. リポジトリのSecretsに`PERSONAL_ACCESS_TOKEN`を設定
3. `main`ブランチにプッシュして動作確認

## 🎨 テーマ

このサイトは[LoveIt](https://github.com/dillonzq/LoveIt)テーマを使用しています。

テーマの更新：
```bash
git submodule update --remote --merge
```

## 📚 参考資料

- [Hugo公式ドキュメント](https://gohugo.io/documentation/)
- [LoveItテーマドキュメント](https://hugoloveit.com/)
- [GitHub Pages](https://docs.github.com/pages)

## 📄 ライセンス

コンテンツ: CC BY-NC 4.0
