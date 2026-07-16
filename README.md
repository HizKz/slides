# slides

Markdownで作成したスライドの編集元をまとめて管理するリポジトリです。

## スライド

- [`go-lesson/`](go-lesson/README.md): Go初心者向け講義スライド
- [`Karabiner/`](Karabiner/README.md): Karabiner-Elements入門スライド

## 管理方針

- Markdown、README、画像素材、生成に必要なNode.js設定をGitで管理する
- HTML、PDF、PowerPoint、プレビュー画像などの生成物はGitに含めない
- 依存関係は各スライドのディレクトリでインストールする

```sh
cd go-lesson
npm install
```

```sh
cd Karabiner
npm install
```

生成方法の詳細は各ディレクトリのREADMEを参照してください。
