# 開発環境を WSL に置く — 何をしたか / なぜか

> **鳥用 3 行まとめ**
> 1. WSL = Windows の中で本物の Linux が動く仕組み。本番・CI・Docker と同じ OS で開発できる
> 2. ソースは **WSL 側（`~/workspace`）** に置く。`/mnt/c/...` 経由は遅い
> 3. ターミナルは「Ubuntu」を開く。VS Code はそこから `code .`

---

## なぜ WSL？（Java で例えると）

開発機 Windows・本番 Linux で WAR を動かして「改行コードで死んだ」「パスの `\` で死んだ」経験があるはず。
今どきは **開発機も Linux に揃える** のが普通。CI（GitHub Actions）も Docker も Linux で動くので、手元と同じ挙動になる。

## 実施したコマンドと意味

すべて WSL の Ubuntu ターミナルで実行（または Windows から `wsl -d Ubuntu-24.04 -- ...`）。

### 1. Git に「自分は誰か」を教える

```bash
git config --global user.name  caicats00876
git config --global user.email caicats00876@gmail.com
```

- コミットに記録される作者名。WSL の Git は Windows の Git とは**別インストール**なので、設定も別。
- `--global` = この Linux ユーザー全体の設定（`~/.gitconfig` に書かれる）。

### 2. 改行コードの扱いを決める

**方針：Windows で作業するときも含めて、改行は LF で統一する。Git には変換させない。**

```bash
git config --global core.autocrlf false   # WSL でも Windows の Git でも同じ設定にする
```

- Windows は改行が `CRLF`、Linux は `LF`。混ざると diff が全行変更扱いになったり、シェルスクリプトが動かなくなる（`/bin/bash^M: bad interpreter`）。
- `autocrlf` は Git が勝手に改行を変換する機能。`false` = **変換しない**。ファイルに書かれている改行がそのまま保存される。
  - Windows 版 Git の初期値は `true`（取り出すときに CRLF に変換）。これを使うと Windows 側だけ CRLF になるので切った。

`autocrlf` は **各自の PC の設定** なので、設定し忘れた人がいると崩れる。そこでリポジトリ側にも 2 つ置いた：

| ファイル | 誰に効く | 何をしている |
|---|---|---|
| `.gitattributes` | Git | `* text=auto eol=lf` → 誰がどの OS で clone しても、テキストは LF で取り出し・LF で保存。`autocrlf` より優先される |
| `.editorconfig` | エディタ | `end_of_line = lf` → VS Code 等で新規作成・保存したファイルを LF にする |

Java で言うと、`autocrlf` は各自の IDE 設定、`.gitattributes` / `.editorconfig` はプロジェクトに入れる `pom.xml` の `<project.build.sourceEncoding>` みたいなもの。**個人設定に頼らず、リポジトリで強制する**のが今どきのやり方。

> VS Code の右下ステータスバーに `LF` と出ていれば OK。`CRLF` と出ていたらクリックで切り替えられる。

### 3. Windows 側のリポジトリを WSL 側に複製

```bash
git clone /mnt/c/git/py-web-app ~/workspace/py-web-app
```

- `/mnt/c/` = WSL から見た Windows の C ドライブ。
- `~/workspace/` = Linux 自身のディスク（ext4）。**こっちの方が桁違いに速い**（`npm install` で数万ファイル書くので効く）。
- `clone` なのでコミット履歴ごと移る。ファイルコピーより安全。

### 4. 接続先の名前を変える

```bash
git remote rename origin windows-copy
git remote -v
```

- clone 元は自動で `origin` という名前で登録される。今は Windows 側のフォルダが origin になっている。
- origin は普通「GitHub」を指す名前なので、紛らわしくないよう `windows-copy` に改名して空けておく。
- GitHub のリポジトリができたら：

```bash
git remote add origin https://github.com/<ユーザー名>/py-web-app.git
git push -u origin main
git remote remove windows-copy   # もう不要
```

## WSL に最初から入っていたもの

| ツール | バージョン | 何者か |
|---|---|---|
| Python | 3.12 | バックエンドの言語。JDK に相当 |
| uv | — | Python の依存管理 + 仮想環境。Maven + JDK 切替を 1 つにした感じ |
| Node.js | 22 | JavaScript の実行環境。React のビルドに使う（ブラウザで動くのは別） |

## まだ入っていないもの

- **Docker**（07章で使う）：Docker Desktop の「WSL Integration」を ON にするか、Ubuntu に Docker Engine を直接入れる。

## 毎日の始め方

```bash
cd ~/workspace/py-web-app
code .          # VS Code が WSL モードで開く（左下に「WSL: Ubuntu-24.04」と出れば OK）
```
