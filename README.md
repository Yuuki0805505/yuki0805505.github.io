# Create a full-stack prototype repo for "Democracy Change Platform"
import os, json, textwrap, zipfile, pathlib

root = "/mnt/data/democracy-change-platform"
server = f"{root}/server"
client = f"{root}/client"

os.makedirs(server + "/src", exist_ok=True)
os.makedirs(client + "/src", exist_ok=True)
os.makedirs(client + "/public", exist_ok=True)

# Root README
readme = """
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
