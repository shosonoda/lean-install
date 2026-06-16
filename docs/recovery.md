# リカバリー手順書

ここでは，環境構築でよくある失敗と復旧手順をまとめます．

まず大原則です．

1. エラーメッセージを消す前にコピーまたはスクリーンショットを保存する．
2. いきなり全部を再インストールしない．
3. プロジェクトルート，つまり `lean-toolchain` と `lakefile.toml` があるフォルダで実行しているか確認する．
4. Windows では，ターミナルを閉じて開き直すだけで PATH 問題が直ることがあります．

## 0. `git`, `curl`, `code` が見つからない

症状:

```text
git: command not found
curl: command not found
code: command not found
```

または Windows で:

```text
'git' is not recognized as an internal or external command
'curl' is not recognized as an internal or external command
'code' is not recognized as an internal or external command
```

対処:

1. ターミナルを閉じて開き直す．
2. `git` / `curl` は [作業マニュアル 2](setup-manual.md#2-git-curl) に戻る．
3. `code` は [作業マニュアル 1](setup-manual.md#1-vs-code) に戻る．

Windows で `code` だけ見つからない場合は，VS Code を起動してコマンドパレットから `Shell Command: Install 'code' command in PATH` を実行するか，インストール時の `Add to PATH` 設定を確認してください．

急ぐ場合は，`code .` の代わりに VS Code の `File > Open Folder...` からプロジェクトフォルダを開いてください．

## 1. `gh` が見つからない

症状:

```text
gh: command not found
```

または:

```text
'gh' is not recognized as an internal or external command
```

対処:

1. GitHub CLI をインストールする．
2. ターミナルを閉じて開き直す．
3. 次を確認する．

```bash
gh --version
```

急ぐ場合は，GitHub CLI を使わずに HTTPS で clone します．

```bash
git clone https://github.com/shosonoda/lean-math-note.git
```

## 2. GitHub の SSH まわりで失敗する

症状の例:

```text
Permission denied (publickey).
Could not read from remote repository.
Host key verification failed.
```

講義では SSH を使わず，HTTPS に寄せるのが簡単です．

### 2.1 GitHub CLI のプロトコルを HTTPS にする

```bash
gh config set git_protocol https
gh auth login
```

ログイン時に次を選びます．

- GitHub.com
- HTTPS
- Authenticate Git with your GitHub credentials: Yes

確認:

```bash
gh config get git_protocol
gh auth status
```

### 2.2 clone 済みリポジトリの remote を HTTPS に変える

プロジェクトフォルダで:

```bash
git remote -v
```

`git@github.com:...` のように SSH になっていたら，HTTPS に変えます．

```bash
git remote set-url origin https://github.com/shosonoda/lean-math-note.git
git remote -v
```

### 2.3 Git の global 設定で HTTPS が SSH に書き換えられていないか見る

一部の環境では，HTTPS URL を自動で SSH に書き換える設定が入っていることがあります．

```bash
git config --global --get-regexp url
```

`insteadOf` で GitHub の URL が SSH に書き換えられている場合は，講師に相談してください．自分で消す場合は，消す対象を必ず確認してから行います．

例:

```bash
git config --global --unset-all url.git@github.com:.insteadof
```

!!! warning "設定削除は慎重に"
    研究室や会社の PC では，Git 設定が意図的に管理されていることがあります．共有 PC や管理 PC では勝手に削除せず，管理者または講師に確認してください．

## 3. `elan`, `lean`, `lake` が見つからない

症状:

```text
elan: command not found
lean: command not found
lake: command not found
```

または Windows で:

```text
'elan' is not recognized as an internal or external command
```

### 3.1 まずターミナルを開き直す

インストール直後は PATH が反映されていないことがあります．ターミナルを完全に閉じて，新しく開いてください．

確認:

Windows:

```powershell
where elan
where lean
where lake
```

macOS / Linux:

```bash
which elan
which lean
which lake
```

### 3.2 Windows で手動インストールする

PowerShell または Command Prompt で:

```powershell
curl -O --location https://elan.lean-lang.org/elan-init.ps1
powershell -ExecutionPolicy Bypass -f elan-init.ps1
del elan-init.ps1
```

選択肢が出たら `1` を選びます．

終わったらターミナルを開き直し，確認します．

```powershell
elan --version
lean --version
lake --version
```

### 3.3 `curl` も PowerShell スクリプトも失敗する場合

ブラウザから手動でインストーラを入れる方法があります．

1. ブラウザで `https://github.com/leanprover/elan/releases` を開く．
2. 最新リリースを開く．
3. Windows なら `x86_64-pc-windows-msvc` を含む zip を選ぶ．
4. zip を展開する．
5. 中にある `elan-init.exe` を実行する．
6. 画面の指示に従い，通常は `1` を入力する．
7. ターミナルを開き直して確認する．

```powershell
elan --version
lean --version
lake --version
```

!!! note "ARM Windows の場合"
    ARM 版 Windows など特殊な環境では，ファイル名が異なる場合があります．リリースページで自分の OS / CPU に合ったものを選んでください．

## 4. `lake exe cache get` が失敗する

症状の例:

```text
error: external command ... exited with code ...
network error
could not resolve host
SSL certificate problem
Permission denied (publickey)
```

### 4.1 プロジェクトルートにいるか確認する

Windows:

```powershell
Get-ChildItem
Get-Content lean-toolchain
```

macOS / Linux:

```bash
ls
cat lean-toolchain
```

`lean-toolchain` と `lakefile.toml` が見えない場合，フォルダが違います．

### 4.2 ネットワークを確認する

```bash
curl -I https://github.com
curl -I https://elan.lean-lang.org/elan-init.ps1
```

大学・会社・公共 Wi-Fi では，GitHub や外部バイナリのダウンロードが制限されることがあります．可能なら別回線，テザリング，またはプロキシ設定を確認してください．

### 4.3 SSH エラーなら HTTPS に寄せる

このページの「GitHub の SSH まわりで失敗する」を実行します．

### 4.4 キャッシュを取り直す

まず軽い復旧から試します．

```bash
lake clean
lake exe cache get!
lake build
```

`get!` は，ローカルに既にあると見なされているキャッシュも取り直すためのコマンドです．

## 5. `lake build` が Mathlib を長時間ビルドし始める

症状:

- PC のファンが回り続ける．
- `Mathlib...` が大量にビルドされる．
- 何十分も終わらない．

原因候補:

- `lake exe cache get` が失敗していた．
- キャッシュの展開に失敗していた．
- `.lake` が古い状態のまま残っている．

対処:

```bash
lake clean
lake exe cache get!
lake build
```

まだダメなら `.lake` を削除します．

Windows PowerShell:

```powershell
Remove-Item -Recurse -Force .lake
lake exe cache get
lake build
```

Windows Command Prompt:

```cmd
rmdir /s /q .lake
lake exe cache get
lake build
```

macOS / Linux:

```bash
rm -rf .lake
lake exe cache get
lake build
```

## 6. Lean / Mathlib のバージョン違いで壊れる

症状の例:

```text
unknown package
unknown module prefix 'Mathlib'
invalid manifest
package configuration has changed
error: toolchain ... does not have the binary ... lake
```

### 6.1 勝手に `lean-toolchain` を変えない

Lean プロジェクトでは，`lean-toolchain` がそのプロジェクトの Lean バージョンを指定します．

講義用リポジトリでは，受講者が `elan default stable` や `lean-toolchain` の編集で直そうとすると，逆に壊れることがあります．

まず，配布リポジトリの状態に戻します．

```bash
git status --short
git restore lean-toolchain lakefile.toml lake-manifest.json
git pull
```

`git restore` が失敗する場合は，変更を講師に見せてください．

### 6.2 `.lake` を消して取り直す

```bash
lake clean
```

それでもダメなら:

Windows PowerShell:

```powershell
Remove-Item -Recurse -Force .lake
lake exe cache get
lake build
```

macOS / Linux:

```bash
rm -rf .lake
lake exe cache get
lake build
```

### 6.3 `lake-manifest.json` について

`lake-manifest.json` は，単なる一時キャッシュではなく，依存パッケージのバージョンを固定する重要ファイルです．

講義用リポジトリでは，原則として受講者が削除・再生成しない方が安全です．講師から「manifest を再生成してください」と指示があった場合だけ，次を実行します．

```bash
rm -f lake-manifest.json
lake update
lake exe cache get
lake build
```

Windows PowerShell の場合:

```powershell
Remove-Item -Force lake-manifest.json
lake update
lake exe cache get
lake build
```

!!! warning "`lake update` の意味"
    `lake update` は依存関係を更新します．プロジェクトの状態が講義用の固定状態から変わる可能性があります．受講者個人の判断ではなく，講師の指示で実行してください．

### 6.4 Lean toolchain 自体が壊れている場合

`lean-toolchain` の中身を確認します．

Windows:

```powershell
Get-Content lean-toolchain
```

macOS / Linux:

```bash
cat lean-toolchain
```

例えば `leanprover/lean4:v4.xx.x` のような文字列が出ます．

その toolchain を入れ直します．以下の `<TOOLCHAIN>` を実際の文字列に置き換えてください．

```bash
elan toolchain uninstall <TOOLCHAIN>
elan toolchain install <TOOLCHAIN>
lake build
```

## 7. VS Code で Lean が動かない

症状:

- `.lean` ファイルがただのテキストとして開く．
- Lean Infoview が出ない．
- `import Mathlib` が赤い．
- コマンドラインでは `lake build` が成功するのに VS Code だけ失敗する．

対処:

1. VS Code で開いているフォルダがプロジェクトルートか確認する．
   - 正: `lean-math-note`
   - 誤: その親フォルダ `LeanProjects`
2. `File > Open Folder...` で `lean-math-note` を開き直す．
3. `Trust authors` を選ぶ．
4. `.lean` ファイルを開く．
5. コマンドパレットで `Lean 4: Restart File` を実行する．
6. それでもダメなら VS Code を再起動する．

確認用ファイル:

```lean
import Mathlib

#eval 1 + 1

example : 1 + 1 = 2 := by
  norm_num
```

## 8. Markdown プレビューが開かない

症状:

- `.md` ファイルがプレビューできない．
- ショートカットが効かない．

対処:

1. `.md` ファイルを VS Code で開く．
2. コマンドパレットを開く．
3. `Markdown: Open Preview to the Side` を実行する．
4. それでもダメなら VS Code を再起動する．
5. 必要なら `Markdown All in One` 拡張機能を入れる．

## 9. それでも直らない場合の最小再構築

講義中に時間がない場合は，プロジェクトだけを作り直すのが早いことがあります．

```bash
cd C:\LeanProjects
ren lean-math-note lean-math-note-broken

git clone https://github.com/shosonoda/lean-math-note.git
cd lean-math-note
lake exe cache get
lake build
code .
```

macOS / Linux:

```bash
cd ~/LeanProjects
mv lean-math-note lean-math-note-broken

git clone https://github.com/shosonoda/lean-math-note.git
cd lean-math-note
lake exe cache get
lake build
code .
```

壊れたフォルダは後で調査できます．作業中のファイルがある場合は，削除せずリネームしてください．

## 10. Codespaces を利用する

ローカル PC の復旧が講義時間内に間に合わない場合は，GitHub Codespaces を使ってブラウザ上の VS Code で作業できます．

手順は [Codespaces 利用手順](codespaces.md) を見てください．

注意点:

- GitHub アカウントが必要です．
- ローカル PC の Lean / VS Code インストール状態には依存しません．
- 使い終わった codespace は停止または削除してください．
