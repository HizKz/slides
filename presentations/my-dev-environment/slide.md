---
marp: true
theme: mint
size: 16:9
paginate: true
footer: '私の開発環境'
---

<!-- _class: cover -->
<!-- _footer: '' -->

# 私の開発環境

普段使っているツールと設定

---

# 目次

1. 開発環境の全体像
2. ターミナルとエディタ
3. 操作と設定の管理
4. ブラウザとタスク管理

---

# 開発環境の全体像

<div class="columns">
<div>

## 開発する

WezTerm → Herdr → Zsh

↓

Nixvim ／ Codex

<span class="small">移動・Git操作：ghq・fzf・zoxide・Lazygit</span>

</div>
<div>

## 調べる・予定を確認

Ardia ／ Muse

Notion ↔ TimeBlocker

カレンダー → TimeBlocker

</div>
</div>

<span class="small">入力：Karabiner-Elements ／ 設定管理：Nix・Home Manager・dotfiles</span>

---

# WezTerm：作業するターミナル

<div class="columns">
<div>

## 普段の操作

タブとペインで作業画面を分ける

新しいタブではログインZshが開く

## 操作例

`Cmd + t`：新しいタブ

`Ctrl + z`：ペインを拡大・復元

</div>
<div>

![width:480px](images/wezterm-official.png)

<span class="small">画面例：[WezTerm公式](https://wezterm.org/)</span>

</div>
</div>

---

# ZshとStarship：コマンド入力

- **Zsh**：コマンドを入力するシェル
- 入力中に履歴候補を表示し、構文をハイライト
- 入力済みの文字に一致する履歴を上下キーで検索
- **Starship**：プロンプトに時刻・ディレクトリ・Gitブランチを表示

---

# WezTermとHerdrの役割

| | WezTerm | Herdr |
| --- | --- | --- |
| 役割 | ターミナルのウィンドウ・タブ | 開発作業のセッション |
| 操作 | ターミナルの画面を切り替える | プロジェクト単位のWorkspaceを使う |

シェルから `herdr` を起動し、デタッチ後は再接続できる。

Herdr内のZshからNixvimやCodexを使う。

<span class="small">PCやHerdrサーバーの再起動時は、元のプロセスは終了します。</span>

---

# Herdr：作業をプロジェクトごとに整理

**Workspace → Tab → Pane** の順に作業を分ける

| 単位 | 使い方の例 |
| --- | --- |
| Workspace | プロジェクトごとに分ける |
| Tab | エージェント・サーバー・テストなどの用途 |
| Pane | 個々のプロセスを表示 |

`Ctrl + b` → `q` でデタッチし、`herdr` で再接続。

---

# コードを書く：NixvimとCodex

**Neovimの設定をNixvimで管理**

- 補完・検索・LSP・フォーマットを設定
- Go、TypeScript、Pythonなどに対応
- `Space` → `e` でファイル一覧を開く
- `Space` → `/` でプロジェクト内を検索
- **Codex**：Herdr内で使うコーディングエージェント

<span class="small">キー表記は順番に入力する操作です。</span>

---

# 移動とGit操作

<div class="columns">
<div>

## プロジェクトへ移動

`ghq` でリポジトリを管理

`cproj` で `fzf` の一覧から選択

`zoxide` は `cd` に統合

</div>
<div>

## Gitを操作

`Lazygit` で操作

`delta` で差分を表示

`cproj` の選択時にも<br>Gitの状態をプレビュー

</div>
</div>

---

# ghq・fzf・zoxide：移動を助ける

- **ghq**：Gitリポジトリを決まった場所にまとめる
- **fzf**：候補をあいまい検索して選択する
- **cproj**：ghqの一覧をfzfで選び、選んだ場所へ移動
- **zoxide**：訪問したディレクトリへの移動を補助

<span class="small">この環境ではzoxideを `cd` に統合しています。</span>

---

# Lazygitとdelta：Gitの状態と差分

**Lazygit**：ターミナル上でGitを操作する画面

- ファイルを選んでstage・unstage、コミットなどを操作
- `n / t / k / s` で上下左右に移動するよう設定

**delta**：Gitの差分を見やすく表示

- 行番号を表示し、差分間の移動を有効化

---

# キー操作：Karabiner-Elements

<div class="columns">
<div>

**macOSのキー入力をカスタマイズ**

- o24配列に合わせてキーを設定
- `Space + w` でWezTermを呼び出す

</div>
<div>

![width:490px](images/karabiner-official.png)

<span class="small">[公式の画面例](https://karabiner-elements.pqrs.org/docs/manual/configuration/configure-simple-modifications/)（実際の設定とは異なります）</span>

</div>
</div>

---

# Nix・Home Manager：設定をまとめる

- **Nix**：パッケージや設定を定義する仕組み
- **Home Manager**：ユーザー環境をNixで管理
- **Nixvim**：Neovimの設定やプラグインをNixで定義
- 設定は `home-manager switch --flake .#apple` で反映

<span class="small">以前のHome Manager generationに戻すこともできます。</span>

---

# 環境を管理する：Nix + dotfiles

| 管理方法 | 対象 |
| --- | --- |
| Nix + Home Manager | Zsh、CLIツール、Nixvimなど |
| シンボリックリンク | WezTerm、Karabinerなどの設定 |
| 別の手段 | GUIアプリや言語ランタイム |

`dotfiles check` で設定とリンクを検証し、<br>`dotfiles doctor` で読み込み先などを診断。

---

# Ardia：普段使うブラウザ

**Arcライクなブラウザ**

- WebKitとChromiumを切り替えて使える
- 開発中のWebページを異なるエンジンで確認できる

---

# Muse：AIエージェントによるタスク管理

**Moodle・Googleカレンダー・メールなどの情報**

↓

**MuseがNotionにタスクを自動作成・管理**

NotionのタスクはTimeBlockerからも確認できる。

---

# TimeBlocker：予定とタスクを確認

<div class="columns">
<div>

**macOSのメニューバーから開くアプリ**

- 予定とNotionのタスクを一覧表示
- 予定追加・タスク追加と完了
- 予定の開始やタスクの期限を通知

<span class="small">Notionは約60秒ごとに同期。通知には期限時刻の設定が必要。</span>

</div>
<div>

![width:250px](images/timeblocker-icon.png)

<span class="small">TimeBlockerのアプリアイコン</span>

</div>
</div>

---

# まとめ

- **WezTerm → Herdr → Zsh**：開発作業の実行環境
- **NixvimとCodex**：コード編集とエージェントによる開発
- **Nixとdotfiles**：ツールや設定を管理する
- **Ardiaとタスク管理ツール**：調べることや予定の確認を支える

<span class="small">参考：[dotfiles](https://github.com/HizKz/dotfiles) ／ [TimeBlocker](https://github.com/HizKz/TimeBlocker)</span>

---

<!-- _class: closing -->
<!-- _footer: '' -->

# ありがとうございました
