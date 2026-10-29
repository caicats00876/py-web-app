# 00. Git と GitHub — 作業フォルダを同期する

> **鳥用 3 行まとめ**
> 1. `git add` → `git commit` で **手元に** 保存、`git push` で **GitHub に** 送る
> 2. GitHub は「Git リポジトリ置き場 + レビュー + CI 実行基盤」
> 3. 秘密情報（`.env`）は `.gitignore` に書いて絶対にコミットしない

---

## 1. SVN 経験者向け：Git は何が違う？

| | SVN | Git |
|---|---|---|
| リポジトリ | サーバに 1 つ | **全員の PC に丸ごと 1 つずつ** |
| commit | サーバに送る | **手元に**記録するだけ |
| サーバに送る | （commit と同時） | `git push` |
| 最新を取る | `svn update` | `git pull` |

つまり **commit と push は別物**。commit はセーブ、push はクラウドにアップロード。

```
 作業フォルダ ──add──▶ ステージ ──commit──▶ ローカルリポジトリ ──push──▶ GitHub
   (編集中)          (次に保存する候補)       (.git フォルダ)            (origin)
                                                         ◀──pull───
```

## 2. この章でやったこと（実行済み）

```bash
git init -b main          # このフォルダを Git 管理下にする（.git フォルダができる）
git add .                 # 全ファイルを「次に保存する候補」に
git commit -m "..."       # 手元に保存
```

`.gitignore` には「Git に入れないもの」を書いた：
- `node_modules/`, `.venv/` … 依存ライブラリ。Maven の `~/.m2` みたいなもの。再取得できるので入れない
- `.env` … パスワードや API キー。**入れたら事故**

## 3. GitHub とつなぐ（あなたがやる手順）

1. GitHub で空のリポジトリを作る：<https://github.com/new>
   - Repository name: `py-web-app`
   - **README / .gitignore / License は追加しない**（手元に既にあるので衝突する）
2. 表示された URL を使って：

```bash
git remote add origin https://github.com/<ユーザー名>/py-web-app.git
git push -u origin main
```

- `remote add origin` … 「origin という名前で GitHub の場所を登録」
- `push -u` … 初回だけ `-u`。以後は `git push` だけでいい

## 4. 毎日使うのはこれだけ

```bash
git status                # いま何が変わってる？（迷ったらまずこれ）
git add .                 # 変更を全部候補に
git commit -m "やったこと" # 手元に保存
git push                  # GitHub に送る
git pull                  # GitHub の最新を取り込む
git log --oneline         # 履歴を見る
```

## 5. ブランチ（06 章の CI で効いてくる）

`main` を直接いじらず、作業用の枝を切る → GitHub で **Pull Request (PR)** → レビュー → main に合流。
CI は「PR を出した瞬間に自動テストして ✅ / ❌ をつける係」。詳しくは 06 章で。

```bash
git switch -c feature/hello-api   # 枝を作って移動
# ...作業して commit...
git push -u origin feature/hello-api
```
