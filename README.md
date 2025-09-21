
# 民主主義変化プラットフォーム（擬似国会）

コメントを使って意見交換し、法案（モーション）を段階的に審議・修正・投票できる**擬似国会**のプロトタイプです。  
ローカルで動くフルスタック構成（React + Vite / Express + SQLite）。GitHub へのプッシュだけで利用開始できます。

## 主要機能（MVP）
- ユーザー登録（ニックネームのみ・簡易認証）
- モーション（法案）の作成・閲覧
- 審議ステージ（`proposal` → `committee` → `plenary` → `vote`）の進行
- スレッド式コメント（各ステージごと）
- 賛成/反対/棄権の**会派別カウント**（与党/野党を任意で設定）
- ライブ更新（SSE）でコメントと投票が即時反映
- モデレーター（スピーカー）によるステージ進行

## すぐに動かす

### 1) 依存をインストール
```bash
# root では何もしない想定（サブパッケージごと）
cd server && npm i && cd ../client && npm i
```

### 2) サーバーを起動（ポート: 8787）
```bash
cd server
npm run dev
```

### 3) クライアントを起動（ポート: 5173）
```bash
cd client
npm run dev
```

### 4) ブラウザでアクセス
- フロントエンド: http://localhost:5173
- API: http://localhost:8787

> 初回起動時に `server/data.db` が自動生成されます。

## デプロイのヒント
- **Server（Express）**: Render / Railway / Fly.io など（`PORT` 環境変数対応）
- **Client（Vite）**: Vercel / Netlify など（`VITE_API_BASE` をAPI URLに設定）
- **Docker**: 下の compose 例を参照

## システム設計（概要）
- **サーバー**: Express + better-sqlite3。REST API + SSE エンドポイント。
- **DB**: SQLite（単一ファイル）。テーブル: `users`, `motions`, `comments`, `votes`, `stages`。
- **フロント**: React + Tailwind。ロビー/モーション詳細/モデレーターUI。

## Docker（任意）
```yaml
# docker-compose.yml（例）
services:
  api:
    build: ./server
    ports: ["8787:8787"]
    environment:
      - PORT=8787
  web:
    build: ./client
    ports: ["5173:5173"]
    environment:
      - VITE_API_BASE=http://localhost:8787
```
