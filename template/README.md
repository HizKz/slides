# Mint Marp テンプレート

ミントと白を基調に、余白と文字を大切にした発表用テンプレートです。

フォントには Zen Kaku Gothic New を使用します。表示・出力する端末にインストールしてください。未インストールの場合は代替フォントで表示されます。

## 使い方

1. `template.md` をコピーして、発表用の Markdown ファイルを作ります。
2. タイトル、名前、日付、本文、フッターを差し替えます。不要なスライドは区切りの `---` とあわせて削除し、必要なスライドはコピーして使えます。
3. ターミナルでこの `template` ディレクトリに移動し、Marp CLI で出力します（[Marp CLI](https://github.com/marp-team/marp-cli) が必要です）。

```sh
marp --html --theme themes/mint.css template.md -o slides.html
marp --html --theme themes/mint.css template.md --pdf -o slides.pdf
```

PDF の生成には Marp CLI が利用できるブラウザーが必要です。`--html` は2カラムなどの HTML を有効にします。信頼できる Markdown ファイルに対して使ってください。

VS Code の Marp for VS Code 拡張機能でも利用できます。開いているワークスペースのルートが `slides` の場合、設定 `markdown.marp.themes` に `./template/themes/mint.css` を登録し、プレビューを開いてください。`template` をルートとして開いた場合は `./themes/mint.css` です。HTML を使用するには `markdown.marp.enableHtml` も有効にします。

## スライドの種類

- 表紙： `<!-- _class: cover -->`
- 目次：表紙の次に配置し、発表の構成や順序に合わせて項目を編集します。
- 章扉： `<!-- _class: divider -->`
- 本文：クラス指定なし
- 2カラム：サンプルの `<div class="columns">` をコピーします。Markdown の前後の空行も残してください。
- 締め： `<!-- _class: closing -->`

表紙・章扉・締めは `<!-- _footer: '' -->` でフッターを消せます。色や余白は `themes/mint.css` で調整できます。

## デザインの参考

余白による強調と、情報を絞ったスライドづくりの考え方を参考にしています。配置や図版のコピーはしていません。

- [余白を恐れないデザイン術（Speaker Deck）](https://speakerdeck.com/keita_yoshikawa/fu-shan-tesainmian-qiang-hui-yu-bai-wokong-renaitesainshu)
- [DrupalCamp Speaking 101（Speaker Deck）](https://speakerdeck.com/ryan_chartrand/drupalcamp-speaking-101)
