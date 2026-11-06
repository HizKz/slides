# 15日間のエンジニアインターンで学んだこと

インターンでの取り組みと学びを紹介するスライドです。

- [編集元](intern-15days.marp.md)
- `images/`：スライドで使用する画像

## 生成

Marp CLIを用意し、リポジトリのルートから実行します。PDFの生成には対応ブラウザーが必要です。

```sh
marp --html --allow-local-files --theme template/themes/mint.css presentations/intern-15days/intern-15days.marp.md -o presentations/intern-15days/intern-15days.html
marp --html --allow-local-files --theme template/themes/mint.css presentations/intern-15days/intern-15days.marp.md --pdf -o presentations/intern-15days/intern-15days.pdf
```

生成物はGitの管理対象外です。
