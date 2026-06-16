# 作業マニュアル

このページでは，講義で使う Lean 環境をゼロから準備します．

以下のGitHubリポジトリを導入します:

```bash
gh repo clone shosonoda/lean-math-note
```

GitHub CLI を使わない場合は，HTTPS の `git clone` でも構いません．

```bash
git clone https://github.com/shosonoda/lean-math-note.git
```

GitHub アカウントを持っていない場合でも，公開リポジトリをローカルに clone して講義用プロジェクトを実行するところまでは進められます．その場合は GitHub CLI のログイン設定をスキップし，HTTPS の `git clone` を使ってください．

## 0. 作業フォルダを作る

Windows の例:

```powershell
mkdir C:\LeanProjects
cd C:\LeanProjects
```

macOS / Linux の例:

```bash
mkdir -p ~/LeanProjects
cd ~/LeanProjects
```

!!! warning "フォルダ名について"
    Lean や Lake そのものは日本語パスを必ず禁止しているわけではありませんが，講義中のトラブルを減らすため，作業フォルダ名には英数字・ハイフン・アンダースコアだけを使うことを推奨します．

## 1. VS Code をインストールする

1. [VS Code 公式サイト](https://code.visualstudio.com/) からインストーラを入手します．
2. Windows では，インストール時に可能なら次の項目を有効にします．
    - `Add to PATH`
    - `Register Code as an editor for supported file types`
3. インストール後，ターミナルを新しく開いて確認します．

```bash
code --version
```

`code` が見つからない場合は，VS Code を一度起動し，コマンドパレットから `Shell Command: Install 'code' command in PATH` を実行するか，OS の PATH 設定を確認してください．

## 2. Git と curl を確認する

Lean の依存関係取得や GitHub リポジトリの clone には `git` と `curl` が必要です．

```bash
git --version
curl --version
```

### Windows

`git` がない場合は Git for Windows をインストールします．インストール中にエディタを選ぶ画面が出たら，VS Code を選ぶのがおすすめです．

`curl` は Windows 10/11 には通常入っています．見つからない場合は，Git for Windows 付属の Git Bash を使うか，curl を別途インストールします．

### macOS

```bash
git --version
```

を実行すると，Xcode Command Line Tools のインストールを促される場合があります．画面の案内に従ってください．

### Ubuntu Linux

```bash
sudo apt update
sudo apt install git curl
```

## 3. VS Code に Lean 4 拡張機能を入れる

- 拡張機能名称: Lean 4 VS Code Extension
- item name: `leanprover.lean4`
- 発行者: leanprover

### （推奨）公式マーケットプレイスからインストール

1. [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=leanprover.lean4) からインストールできます．

### VS Code からインストール

1. VS Code を開きます．

2. 左側の Extensions を開く．ショートカットは Windows/Linux で `Ctrl+Shift+X`，macOS で `Cmd+Shift+X`．
3. `lean4` と検索する．
4. 発行者が `leanprover` の **Lean 4** 拡張機能をインストールする．
5. Welcome / Setup Guide が表示されたら，案内に従う．

Setup Guide が自動で開かない場合は，新しい空のファイルを作って，右上の `∀` アイコンから `Documentation > Show Setup Guide` を開けます．

## 4. elan と Lean を入れる

`elan` は Lean のバージョン管理ツールです．
<!-- プロジェクトの `lean-toolchain` ファイルを読み，必要な Lean バージョンを自動的に選びます． -->

### （推奨） VS Code Lean 4 拡張の Setup Guide から入れる

- Lean 4 拡張機能の Setup Guide に `Install Lean using Elan` などの案内が出る場合は，それに従います
- [VS Code Setup Guide](vscode://leanprover.lean4/setup-guide) が開ける場合は，それに従います

### 手動インストール: Windows

新しい PowerShell または Command Prompt を開き，次を実行します．

```powershell
curl -O --location https://elan.lean-lang.org/elan-init.ps1
powershell -ExecutionPolicy Bypass -f elan-init.ps1
del elan-init.ps1
```

途中で選択肢が出たら，通常は `1` を入力してデフォルト設定で進めます．

終わったら，ターミナルを閉じて開き直します．

```powershell
elan --version
lean --version
lake --version
```

### 手動インストール: macOS / Linux

```bash
curl https://elan.lean-lang.org/elan-init.sh -sSf | sh
source $HOME/.elan/env
```

確認します．

```bash
elan --version
lean --version
lake --version
```

!!! note "`lean --version` の初回実行"
    初回は Lean 本体のダウンロードが始まることがあります．数分待ってください．

## 5. （オプション） GitHub CLI を設定する

この機能を使うには GitHub アカウントが必要です．
ターミナルで `git` が使えれば，GitHub CLI (`gh`) は必須ではありません．

<!-- GitHub CLI を使うと次の形で clone できます．

```bash
gh repo clone shosonoda/lean-math-note
``` -->

GitHub CLI が入っているか確認します．

```bash
gh --version
```

未インストールなら GitHub CLI をインストールしてください．

インストール後，ログインします．

```bash
gh auth login
```

画面の質問では，講義用途では次の選択がおすすめです．

- 接続先: `GitHub.com`
- Git 操作のプロトコル: `HTTPS`
- GitHub 資格情報で Git を認証するか: `Yes`
- 認証方法: ブラウザ認証

HTTPS を選んでおくと，Windows の SSH 鍵まわりのトラブルを避けやすくなります．

設定確認:

```bash
gh auth status
gh config get git_protocol
```

`git_protocol` が `https` であれば OK です．

### GitHub アカウントを持っていない場合

GitHub アカウントがなくても，公開リポジトリの clone には HTTPS URL を使えます．この場合は，この節の `gh auth login` は実行しません．

次の節では，GitHub CLI ではなく `git clone` の手順を使ってください．

```bash
git clone https://github.com/shosonoda/lean-math-note.git
```

GitHub アカウントが必要になるのは，主に次の場合です．

- GitHub CLI でログインして操作したい場合
- GitHub Codespaces を使う場合
- 自分の GitHub リポジトリに変更を保存・push したい場合

講義中にローカル環境の構築が間に合わず Codespaces を利用する場合は，GitHub アカウントが必要です．手順は [Codespaces 利用手順](codespaces.md) を見てください．

## 6. 講義用リポジトリを clone する

以下のGitHubリポジトリを clone します:

- [https://github.com/shosonoda/lean-math-note/](https://github.com/shosonoda/lean-math-note/)

GitHub CLI を使う場合:

```bash
cd C:\LeanProjects   # Windows 例
# または cd ~/LeanProjects  # macOS/Linux 例

gh repo clone shosonoda/lean-math-note
cd lean-math-note
```

GitHub CLI を使わない場合，または GitHub アカウントを持っていない場合:

```bash
git clone https://github.com/shosonoda/lean-math-note.git
cd lean-math-note
```

ファイルを確認します．

Windows PowerShell:

```powershell
Get-ChildItem
Get-Content lean-toolchain
```

macOS / Linux:

```bash
ls
cat lean-toolchain
```

`lean-toolchain`, `lakefile.toml`, `lake-manifest.json` が見えていれば，Lean プロジェクトとして開く準備ができています．

## 7. Mathlib のビルド済みキャッシュを取得する

プロジェクトのルート，つまり `lean-toolchain` と `lakefile.toml` があるフォルダで実行します．

```bash
lake exe cache get
```

このコマンドは，Mathlib のビルド済みファイルを取得します．これを省略すると，`lake build` が非常に長くなることがあります．

## 8. ビルドする

```bash
lake build
```

エラーなく終了すれば，コマンドライン側の準備は OK です．

## 9. VS Code で開く

プロジェクトのルートで次を実行します．

```bash
code .
```

または VS Code の `File > Open Folder...` から `lean-math-note` フォルダを開きます．

VS Code が `Trust authors?` と聞いてきたら，講義用の配布リポジトリであることを確認してから `Trust` を選びます．

## 10. Lean が VS Code で動くことを確認する

`Test.lean` というファイルを作り，次を書いて保存します．

```lean
import Mathlib

#eval 1 + 1

example : 1 + 1 = 2 := by
  norm_num
```

期待される状態:

- `import Mathlib` に赤い波線が出ない．
- `#eval 1 + 1` の結果が Infoview やツールチップに表示される．
- `example` にエラーが出ない．

!!! tip "赤線が消えるまで待つ"
    初回は Lean サーバーが起動し，依存関係を読み込むまで時間がかかります．数十秒から数分かかることがあります．

## 11. Markdown プレビューを確認する

VS Code で `README.md` または任意の `.md` ファイルを開きます．

- Windows/Linux: `Ctrl+Shift+V`
- macOS: `Cmd+Shift+V`

または，コマンドパレットで `Markdown: Open Preview to the Side` を実行します．

必要に応じて，VS Code 拡張機能 `Markdown All in One` を入れておくと，Markdown 編集が少し楽になります．ただし，プレビュー自体は VS Code 標準機能だけで可能です．

## 12. 推奨 VS Code 拡張機能

必須:

- Lean 4: `leanprover.lean4`

任意:

- Japanese Language Pack for Visual Studio Code: 日本語 UI が必要な場合
- Markdown All in One: Markdown 編集補助
- GitHub Pull Requests: GitHub 連携を VS Code で行う場合
- Even Better TOML: `lakefile.toml` を読みやすくする場合

講義当日は，必須拡張だけ入っていれば進行できます．
