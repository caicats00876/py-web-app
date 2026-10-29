# py-web-app — React × Python で Web サービスを「わかった感じ」になる教材

**対象**: JavaEE バックエンド経験者。REST はわかる。最近の構成には疎い。記憶力は鳥。
**方針**: 動く現物を作りながら、各章「ここだけ覚えればOK」を 3 行で残す。

---

## 完成形（ゴール）

```
[ブラウザ]
   │  React (TypeScript)  ← 画面。JSP/JSF の代わり。ただしブラウザ上で動く
   │  fetch("/api/todos")  JSON
   ▼
[FastAPI (Python)]       ← REST API。JAX-RS の代わり
   │  SQLAlchemy          ← JPA の代わり
   ▼
[DB: SQLite → PostgreSQL]

GitHub に push すると → GitHub Actions がテスト → (後半) 自動デプロイ  ← これが CI/CD
```

## Java 脳 → 今どき 対応表（まずここだけ見ればいい）

| やりたいこと | JavaEE だと | この教材では |
|---|---|---|
| ソース管理 | SVN / Git | Git + GitHub（教材では扱わない。前提知識） |
| ビルド・テストの自動実行 (CI) | Jenkins | GitHub Actions |
| リリース作業 (CD) | 手順書を見て手でデプロイ | GitHub Actions が自動デプロイ |
| ビルド・依存管理 | Maven / Gradle | Python: `uv` ／ フロント: `npm` |
| REST API | JAX-RS (`@Path`, `@GET`) | FastAPI (`@app.get`) |
| DTO + Bean Validation | POJO + `@NotNull` | Pydantic モデル |
| ORM | JPA / Hibernate | SQLAlchemy |
| 画面 | JSP / JSF（サーバで HTML 生成） | React（ブラウザで HTML 生成） |
| AP サーバ | WildFly / WebLogic | Uvicorn（ただのプロセス） |
| テスト | JUnit | pytest ／ Vitest |
| デプロイ | WAR を手で配置 | Docker イメージ + CI/CD で自動 |

## 章立て

| # | 章 | 動くもの | 状態 |
|---|---|---|---|
| 01 | Python 環境と FastAPI で Hello API | `GET /api/hello` が JSON を返す | ← 次ここ |
| 02 | CRUD API と Pydantic / SQLAlchemy | Todo の登録・一覧・更新・削除 | |
| 03 | React 入門（Vite + TypeScript） | 画面に Todo 一覧が出る | |
| 04 | フロントとバックをつなぐ | 画面から登録できる（CORS / プロキシ） | |
| 05 | テストを書く | `pytest` / `vitest` が通る | |
| 06 | CI ってなに | push で GitHub Actions が自動テスト | |
| 07 | Docker でまとめる | `docker compose up` で全部起動 | |
| 08 | CD ってなに | main に入ると自動デプロイ | |

## フォルダ構成（予定）

```
py-web-app/
├─ backend/      # FastAPI
├─ frontend/     # React
├─ docs/         # 各章の解説
└─ .github/workflows/  # CI/CD 定義（06章〜）
```

## 必要なツール

| ツール | 用途 | 確認コマンド |
|---|---|---|
| Git | ソース管理（使い方は前提知識として扱わない） | `git --version` |
| Python 3.12+ | バックエンド | `python --version` |
| Node.js 22 LTS | フロントエンド（03章〜） | `node --version` |
| VS Code | エディタ（推奨） | |
