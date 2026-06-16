# Lean 講義 環境構築ガイド

「数学系エンドユーザーのための Lean 入門」講義の事前準備用ガイドです．

## 概要

- [公式のLeanインストール手順](https://lean-lang.org/install/)に沿って環境構築をします
- [GitHub アカウント](https://github.com/)はなくても動かせます．ただし，
    - バックアップ環境（Codespaces）を利用する際に必要となります
    - アカウント作成・設定には時間がかかりますので，なるべく事前に準備しておいてください
- ローカル環境が構築できなかった場合のバックアップは2通りあります
    - Codespaces を使う（要GitHubアカウント）
    - [LeanPlayground](https://live.lean-lang.org/) を使う（小規模ならこれでも十分）
- Lean はバージョン指定が違うと動きません
    - 講義では安定版（`stable`）を使います
    ```
    leanprover/lean4:v4.30.0
    ```
    - 最新版（`latest`, `nightly`）ではないので，不用意に `update` しないでください
- Mathlib のビルドには時間がかかるので，ビルド済みのキャッシュを取得します
    ```bash
    lake exe cache get   --- まずキャッシュ取得
    lake build           --- そのあとビルド
    ```

## 目標

各自の PC で次ができる状態を目指します

1. 講義資料の [GitHub リポジトリ](https://github.com/shosonoda/lean-math-note) をローカルに clone できる
  ```bash
  git clone https://github.com/shosonoda/lean-math-note.git
  ```
2. VS Code と Lean 4 拡張機能がインストールされている
3. Lean のバージョンマネージャ `elan` が `lean-toolchain` で指定された Lean バージョンを選べる
  ```bash
  cd lean-math-note
  elan show
  ```
4. Mathlib 付き Lean プロジェクトを build できる
  ```bash
  lake exe cache get
  lake build
  ```
5. VS Code で `.lean` ファイルを開くと Lean Infoview が動く．
6. （オプション）VS Code で Markdown のプレビューを表示できる．

## 手順

1. [公式のLeanインストール手順](https://lean-lang.org/install/)に沿って環境構築を試みてください
2. [確認チェックリスト](checklist.md) で足りないものを確認してください
3. 足りないものがあるとき
    - [作業マニュアル](setup-manual.md) に沿ってインストールしてください
4. うまく動かないとき
    - エラーメッセージをよく読んで [リカバリー手順書](recovery.md) を見てください
5. ローカルの環境構築をしない場合
    - [Codespaces 利用手順](codespaces.md) を使って，ブラウザ上の VS Code で Mathlib 付き Lean プロジェクトを動かせます
    - [LeanPlayground](https://live.lean-lang.org/) にコードをコピペして動作確認することもできます

## 想定環境

- OS
    - macOS
    - Windows 10/11  <!-- - Ubuntu Linux など -->
- VS Code
- Lean 4
- Mathlib
- Git / GitHub CLI

!!! note "GitHub アカウントについて"
    公開リポジトリを HTTPS で clone してローカルで実行するだけなら，GitHub アカウントは不要です．GitHub CLI のログイン，Codespaces，自分のリポジトリへの保存には GitHub アカウントが必要です．

!!! tip "作業フォルダ名の注意"
    Windows では，作業フォルダ名に日本語・空白・特殊記号を入れない方が安全です．例: `C:\Users\ユーザー名\LeanProjects` よりも，可能なら `C:\LeanProjects` や `C:\Users\ユーザー名\lean-projects` のようにします．

<!-- ## 参考リンク

- Lean 公式インストール: [https://lean-lang.org/install/](https://lean-lang.org/install/)
- Lean 手動インストール: [https://lean-lang.org/install/manual/](https://lean-lang.org/install/manual/)
- Lean 4 VS Code 拡張: [https://marketplace.visualstudio.com/items?itemName=leanprover.lean4](https://marketplace.visualstudio.com/items?itemName=leanprover.lean4) -->
<!-- - elan: https://github.com/leanprover/elan -->
<!-- - mathlib4: https://github.com/leanprover-community/mathlib4 -->
<!-- - GitHub CLI: https://cli.github.com/ -->
<!-- - GitHub Codespaces: https://docs.github.com/en/codespaces -->
<!-- - GitHub Pages: https://docs.github.com/en/pages -->
