# go-lesson

Go初心者向けの講義スライドを管理するリポジトリです。
各回のスライドは Marp 用の Markdown で作成し、配布・投影用に HTML を生成しています。

## 構成

- `01/`
  - `go-lecture-01.md`: 第1回「Goの基本文法」
  - `go-lecture-01.html`: 生成済みHTML
- `02/`
  - `go-lecture-02.md`: 第2回「関数・slice・map」
  - `go-lecture-02.html`: 生成済みHTML
- `package.json`: Marp CLI 依存関係

## 前提

- Node.js
- npm

依存関係のインストール:

```bash
npm install
```

## スライドの編集

各回の `*.md` を編集します。
Marp のフロントマターと講師メモを含んだ構成になっています。

例:

- `01/go-lecture-01.md`
- `02/go-lecture-02.md`

## HTMLの生成

第1回:

```bash
npx marp 01/go-lecture-01.md -o 01/go-lecture-01.html
```

第2回:

```bash
npx marp 02/go-lecture-02.md -o 02/go-lecture-02.html
```

まとめて再生成する場合:

```bash
npx marp 01/go-lecture-01.md -o 01/go-lecture-01.html
npx marp 02/go-lecture-02.md -o 02/go-lecture-02.html
```

## 運用ルール

- 新しい回を追加するときは `03/` のように番号ディレクトリを切る
- 1つの回につき `Markdown` と生成済み `HTML` を同じディレクトリに置く
- スライドタイトル、説明、スタイルは各 `.md` のフロントマターで管理する
- 講師向けメモは HTML には見えないコメントとして `<!-- ... -->` に書く

## 対象

高専生向けの Go 入門講義を想定しています。
「まず動かす」「少しずつ読む」を重視した、初学者向けの内容です。
