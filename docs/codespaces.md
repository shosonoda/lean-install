# Codespaces 利用手順

ローカル PC へのインストールが講義時間内に間に合わない場合は，GitHub Codespaces を使ってブラウザ上の VS Code で Lean プロジェクトを動かします．

この方法では，あらかじめ `lake exe cache get` 済みの Mathlib プロジェクトをテンプレートから各自の GitHub アカウントへコピーして使います．ローカル PC の Lean / VS Code インストール状態には依存しません．

!!! warning "GitHub アカウントが必要です"
    Codespaces を使うには GitHub アカウントへのログインが必要です．GitHub アカウントを持っていない場合は，先にアカウントを作成してください．アカウントを作れない場合は，講師に相談してください．

## 1. テンプレートを自分のリポジトリにコピーする

ブラウザで次のテンプレート・リポジトリを開きます．

[https://github.com/yuta0x89/lean-course-template](https://github.com/yuta0x89/lean-course-template)

GitHub にログインした状態で，次を実行します．

1. `Use this template` を押す．
2. `Create a new repository` を選ぶ．
3. `Owner` が自分の GitHub アカウントになっていることを確認する．
4. `Repository name` に分かりやすい名前を付ける．例: `lean-course-template-mycopy`
5. 公開範囲は，迷ったら `Private` を選ぶ．
6. `Create repository` を押す．

コピー後に開いているページの URL が，自分のアカウント名を含む形になっていれば OK です．

```text
https://github.com/YOUR-GITHUB-USER/lean-course-template-mycopy
```

!!! note "テンプレートから作る理由"
    テンプレートから作ると，自分用の独立したリポジトリとして始められます．講義中にファイルを編集しても，元のテンプレート・リポジトリには影響しません．

## 2. Codespaces を起動する

自分のコピー先リポジトリのページで，次を実行します．

1. 緑色の `Code` ボタンを押す．
2. `Codespaces` タブを開く．
3. `Create codespace on main` を押す．
4. ブラウザ上で VS Code 風の画面が開くまで待つ．

初回起動には数分かかることがあります．ターミナルが開き，ファイル一覧が見えるまで待ってください．

## 3. Lean プロジェクトとして動くことを確認する

Codespaces のターミナルで，プロジェクトルートにいることを確認します．

```bash
ls
cat lean-toolchain
```

`lean-toolchain`, `lakefile.toml`, `lake-manifest.json` が見えていれば，講義用プロジェクトとして開けています．

このテンプレートは `lake exe cache get` 済みの状態を想定しています．まずは次を実行します．

```bash
lake build
```

成功すれば，講義中の Lean コードは Codespaces 上で編集・実行できます．

もし講師からキャッシュの再取得を指示された場合だけ，次を実行してください．

```bash
lake exe cache get
lake build
```

## 4. VS Code 上で Lean を確認する

Codespaces のファイル一覧から `.lean` ファイルを開くか，`Test.lean` を作って次を書きます．

```lean
import Mathlib

#eval 1 + 1

example : 1 + 1 = 2 := by
  norm_num
```

期待される状態:

- `import Mathlib` に赤い波線が出ない．
- `#eval 1 + 1` の結果が見える．
- `example` にエラーが出ない．

Lean 4 拡張機能が有効になっていない場合は，左側の Extensions で `lean4` を検索し，発行者が `leanprover` の **Lean 4** 拡張機能をインストールしてください．

## 5. Markdown プレビューを確認する

`README.md` を開き，次のどちらかでプレビューを表示します．

- Windows/Linux: `Ctrl+Shift+V`
- macOS: `Cmd+Shift+V`
- コマンドパレット: `Markdown: Open Preview to the Side`

## 6. 作業を保存する

Codespaces 内で作った変更は，Codespaces の中に保存されます．長期的に残すには，自分の GitHub リポジトリへ commit / push します．

画面左の Source Control で:

1. 変更ファイルを stage する．
2. commit message を書く．
3. `Commit` を押す．
4. `Sync Changes` または `Push` を押す．

講義中に保存が必要か分からない場合は，講師に確認してください．

## 7. 講義後に Codespaces を止める

使い終わったら，GitHub の Codespaces 一覧を開きます．

[https://github.com/codespaces](https://github.com/codespaces)

対象の codespace の `...` メニューから `Stop codespace` を選びます．もう不要なら `Delete` して構いません．

!!! warning "利用枠に注意"
    Codespaces の利用条件や無料枠は GitHub 側で変わることがあります．講義後に使わない codespace は止めるか削除してください．

## 8. うまくいかない場合

### `Create codespace` が見つからない

GitHub にログインしているか確認してください．組織アカウントや教育機関の設定によって Codespaces が制限されている場合は，講師に相談してください．

### `lake build` が Mathlib を長時間ビルドし始める

テンプレートのコピー先が正しいか確認してください．それでも長時間ビルドになる場合は，いったん止めて講師に画面を見せてください．

### Lean Infoview が出ない

`.lean` ファイルを開き直し，コマンドパレットから `Lean 4: Restart File` を実行してください．それでも動かない場合は，Codespaces のウィンドウを再読み込みします．

## 9. 参考リンク

- GitHub Docs: [Creating a repository from a template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)
- GitHub Docs: [Quickstart for GitHub Codespaces](https://docs.github.com/en/codespaces/quickstart)
