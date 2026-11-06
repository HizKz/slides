# Goって何がそんなにいいの？

APIサーバーの開発・運用を例に、Goの特徴を紹介するスライドです。

- [編集元](go-why.md)
- `images/`：スライドで使用する画像

## 生成

Marp CLIを用意し、リポジトリのルートから実行します。PDFの生成には対応ブラウザーが必要です。

```sh
marp --html --allow-local-files --theme template/themes/mint.css presentations/go-why/go-why.md -o presentations/go-why/go-why.html
marp --html --allow-local-files --theme template/themes/mint.css presentations/go-why/go-why.md --pdf -o presentations/go-why/go-why.pdf
```

生成物はGitの管理対象外です。
