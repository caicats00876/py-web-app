# CLAUDE.md — この教材リポジトリで作業するときの前提

## 何のリポジトリか

React（フロント）× Python/FastAPI（バック）の Web アプリを、**動く現物を作りながら学ぶ教材**。
章立て・対応表は [README.md](README.md)、環境構築の経緯は [docs/setup-wsl.md](docs/setup-wsl.md)。

## 読者

- JavaEE バックエンド経験者。REST の概念はわかるが、最近のフレームワーク・構成には疎い
- ゴールは「わかった感じになる」こと。網羅より実感
- **記憶力は鳥**：何度でも思い出せる書き方にする

## 書き方のルール

- 各章の冒頭に **「鳥用 3 行まとめ」**（引用ブロック）
- 実行したコマンドには必ず **「具体的に何をしているか」** を添える（1 コマンドずつ）
- Java / JavaEE での対応物を例えに使う（JAX-RS → FastAPI、JPA → SQLAlchemy、Maven → uv/npm など）
- 説明は日本語

## スコープ

- **Git の使い方は扱わない**（前提知識）
- **CI/CD は必ず扱う**（06章 CI = GitHub Actions で自動テスト、08章 CD = 自動デプロイ）

## 環境

- 開発はすべて **WSL2 Ubuntu-24.04** の `~/workspace/py-web-app`
- 入っているもの：Python 3.12 / uv / Node.js 22 / Git。Docker は未導入（07章までに入れる）
- **改行は LF 統一**。`core.autocrlf=false`（WSL・Windows とも）、`.gitattributes` と `.editorconfig` で強制
- VS Code はデスクトップ／スタートメニューの「py-web-app (WSL)」で WSL 接続状態で開く

## 現在地（2026-10-29 時点）

- 済：リポジトリ作成、WSL 移行、LF 統一、README のロードマップ
- 未：**GitHub リモート未接続**。`origin` は空けてあり、clone 元は `windows-copy` という名前で残っている
  - URL をもらったら `git remote add origin <URL>` → `git push -u origin main` → `git remote remove windows-copy`
- 次：01章「Python 環境と FastAPI で Hello API」（`backend/` に作る）
- Windows 側の `C:\git\py-web-app` は移行前の古いコピー。使わない
