# Go初心者向け講義

高専生向けのGo入門講義スライドです。各回をMarp用のMarkdownで管理しています。

## 資料

- [第1回：Goの基本文法](01/go-lecture-01.md)
- [第2回：関数・slice・map](02/go-lecture-02.md)
- [第3回：struct・method・pointer](03/go-lecture-03.md)

## 生成

Marp CLIを用意し、このディレクトリから実行します。各回のHTMLを生成する例です。

```sh
marp 01/go-lecture-01.md -o 01/go-lecture-01.html
marp 02/go-lecture-02.md -o 02/go-lecture-02.html
marp 03/go-lecture-03.md -o 03/go-lecture-03.html
```

PDFを生成するときは `--pdf` を指定し、出力先の拡張子を `.pdf` に変更します。生成物はGitの管理対象外です。
