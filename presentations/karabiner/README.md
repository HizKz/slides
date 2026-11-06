# Karabiner-Elementsを始めよう

Mac初心者〜開発者向けの、約20分のMarpスライドです。

## ファイル

- `karabiner-elements-getting-started.md`: 編集元
- `karabiner-elements-getting-started.html`: ブラウザ投影用
- `karabiner-elements-getting-started.pdf`: 配布用
- `assets/`: Karabiner-Elements公式ドキュメントから引用した画面画像

## 生成

Marp CLIを用意し、このディレクトリから実行します。PDFの生成には対応ブラウザーが必要です。

```sh
marp --html --allow-local-files karabiner-elements-getting-started.md -o karabiner-elements-getting-started.html
marp --html --allow-local-files karabiner-elements-getting-started.md --pdf -o karabiner-elements-getting-started.pdf
```

編集中のプレビュー:

```sh
marp --html --allow-local-files --server .
```

## 内容の参照元

- 個人dotfilesの `karabiner/karabiner.json`
- 個人dotfilesの `karabiner/README.md`
- [Karabiner-Elements公式ドキュメント](https://karabiner-elements.pqrs.org/docs/)
