# slides

発表用スライドの Markdown と画像素材を管理するリポジトリです。

## スライド

| 資料 | 内容 |
| --- | --- |
| [go-lesson](presentations/go-lesson/README.md) | Go初心者向けの連続講義 |
| [karabiner](presentations/karabiner/README.md) | Karabiner-Elements入門 |
| [go-why](presentations/go-why/README.md) | APIサーバーの開発・運用から考えるGoの魅力 |
| [intern-15days](presentations/intern-15days/README.md) | エンジニアインターンで学んだこと |
| [php-cve-部内LT](presentations/php-cve-部内LT/README.md) | PHPのメモリに関する脆弱性の部内LT |

共通の雛形とテーマは [template/](template/README.md) にあります。新しい発表は原則として `presentations/` に専用フォルダを作成します。

## 管理と生成

- Markdown、README、画像素材をGitで管理します。
- HTML、PDF、PowerPoint、プレビュー画像などの生成物はGitに含めません。
- Marp CLIを共通で利用します。各資料のディレクトリで `npm install` を実行する必要はありません。生成方法は各資料のREADMEを参照してください。
- 共通テンプレートは Zen Kaku Gothic New を使用します。未インストールの場合は代替フォントが使われます。
