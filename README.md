# Custom Docmost

## 概要
このプロジェクトは、Docmost を検証、開発、ECRへのプッシュを行うためのソースコードです。

### フォルダ構成

```sh
.
├── apps
│   ├── client               # フロントエンド
│   └── server               # バックエンド
├── packages
├── patches
├── .dockerignore
├── .env                     # 環境変数
├── .env.local.example       # ローカル開発用 環境変数
├── .env.stg.example         # メールテスト用 環境変数
├── .gitignore
├── .npmrc
├── Dockerfile.dev           # ローカル開発、メールテスト用 Dockerfile
├── Dockerfile.prod          # ECR用 Dockerfile
├── LICENSE
├── README.md
├── crowdin.yml
├── docker-compose.local.yml # ローカル開発用 docker-compose.yml
├── docker-compose.stg.yml   # メールテスト用 docker-compose.yml
├── ecr-init.sh              # ECR作成、ECRイメージのプッシュ用
├── ecr-push.sh              # ECRイメージのプッシュ用
├── nx.json
├── package.json
├── pnpm-lock.yaml
└── pnpm-workspace.yaml
```

### コマンド

.envの作成
```bash
# ローカル開発用
cp .env.local.example .env

# メールテスト用
cp .env.stg.example .env
```

APP_SECRETの生成をして、`.env`の`APP_SECRET`に追記
```sh
openssl rand -hex 32
```

コンテナの起動
```sh
# ローカル開発用
docker compose -f docker-compose.local.yml up -d --build

# メールテスト用
docker compose -f docker-compose.stg.yml up -d --build
```

コンテナの起動
```sh
docker compose up -d
```

ボリュームも含めた、コンテナの削除
```sh
docker compose down -v
```

その他コマンド
```sh
# コンテナログの確認(コンテナIDでも可能)
docker compose logs -f docmost

# コンテナ内でbashコマンドを実行する
docker compose exec -it docmost /bin/bash

# 全てのコンテナを停止する
docker container stop $(docker container ls -aq)

# 全てのボリュームを削除する
docker volume rm $(docker volume ls -q)

# システム全体の不要なデータ（停止中のコンテナ、未使用のイメージ、ネットワーク、ボリューム）を削除する
docker system prune --all --volumes --force

# ビルドキャッシュを全て削除する
docker builder prune --all --force

# Docker デーモンによって利用されているディスク総容量に関する情報を表示する
docker system df
```

### アクセス
Docmost
http://localhost:3000

Mailpit
http://localhost:19980

### プロジェクトの共同作業について

このプロジェクトは[GitHub Flow](https://docs.github.com/ja/get-started/using-github/github-flow)に従って、共同作業を行います。

### ブランチの規則

ブランチの一貫性と明確さを保つために、以下の規則を採用しています。

※xxxはIssueの番号を指します。
- feat/issue-xxx 機能追加等
- fix/issue-xxx バグ修正や機能改善等
- refactor/issue-xxx リファクタリング等
- ci/issue-xxx 環境構築に関わる追加や修正等
- chore/issue-xxx その他

### コミットメッセージの規則

コミットメッセージの一貫性と明確さを保つために、[Semantic Commit Message](https://sparkbox.com/foundry/semantic_commit_messages) の規則を採用しています

:wrench: chore: (タスクファイルなどプロダクションに影響のない修正、実稼働のコードの変更は含めない)

    🔧 chore: デバッグ用のログを削除

:memo: docs: (ドキュメントの更新)

    📝 docs: API の使用方法を README に追記

:sparkles: feat: (ユーザー向けの機能の追加や変更)

    ✨ feat: ユーザープロフィール画面の追加

:bug: fix: (ユーザー向けの不具合の修正)

    🐛 fix: ログイン時のエラーハンドリングを修正

:recycle: refactor: (リファクタリングを目的とした修正)

    ♻️ refactor: 変数名を明確にするためのリファクタリング

:art: style: (スタイルやセミコロンの欠落などの修正、実稼働のコードの変更は含めない)

    🎨 style: コードのインデントを修正

:microscope: test: (テストコードの追加や修正、実稼働のコードの変更は含めない)

    🔬 test: 新規登録機能のユニットテストを追加

:construction_worker: ci: (環境構築に関わる追加や修正)

    👷 ci: バージョン変更に伴う Dockerfile の修正
