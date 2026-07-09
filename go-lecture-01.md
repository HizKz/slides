---
marp: true
theme: default
paginate: true
size: 16:9
title: 第1回 Goの基本文法
description: Go初心者向け講義スライド
style: |
  section {
    font-family: "Hiragino Sans", "Yu Gothic", sans-serif;
    font-size: 28px;
    line-height: 1.5;
    padding: 48px;
  }
  h1, h2 {
    color: #0f172a;
  }
  code {
    font-size: 0.9em;
  }
  pre code {
    font-size: 0.8em;
  }
  strong {
    color: #0f766e;
  }
---

<!--
講師メモ:
- 全体を通して「まず動かす」「少しずつ読む」を意識する。
- 細かい仕様よりも、Goの読みやすさと書きやすさを感じてもらう。
-->

# 第1回: Goの基本文法

Go初心者向け  
高専生向け入門

- Goの雰囲気をつかむ
- 最後に簡単なCLIを作る

---

<!--
講師メモ:
- 今日の到達点を最初に明確にする。
- 「全部覚えなくていい、まず読めることが大事」と伝える。
-->

## 今日のゴール

- `package main`、`import`、`main関数` が読める
- 変数、型、`if`、`for`、関数の基本がわかる
- Goで短いプログラムを書ける
- CLIプログラムの入口がイメージできる

---

<!--
講師メモ:
- GoはGoogle製、シンプルさ重視という文脈を軽く説明。
- 他言語経験が少しある学生向けに「C/Java/Pythonの中間っぽい」と伝えてもよい。
-->

## Goとは何か

- 2009年に公開されたプログラミング言語
- シンプルで読みやすい
- コンパイルが速い
- WebサーバーやCLIツールでよく使われる

```go
package main

func main() {
}
```

---

<!--
講師メモ:
- バックエンドとの接続をここで作る。
- 実務での採用理由は「速い」「壊れにくい」「配布しやすい」を中心に。
-->

## Goがバックエンドで使われる理由

- サーバープログラムを書きやすい
- 並行処理が得意
- 1つの実行ファイルにまとめやすい
- コードの書き方がそろいやすい

実務とのつながり:

- APIサーバー
- バッチ処理
- CLI運用ツール

---

<!--
講師メモ:
- この1枚は「まず全体を見る」ためのスライド。
- 次の数枚で1行ずつ分解していくと予告する。
-->

## 最小のGoプログラム

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go")
}
```

- 上から順に読むだけでOK
- まずは「お決まりの形」として見てみる

---

<!--
講師メモ:
- packageは「ファイルのグループ名」とざっくり説明。
- ここでは main package = 実行用、と理解できれば十分。
-->

## `package main`

```go
package main
```

- このファイルは `main` パッケージに属する
- `main` パッケージは「実行できるプログラム」で使う
- Goでは最初に `package` を書く

初心者ポイント:

- `package` を書き忘れるとエラーになる

---

<!--
講師メモ:
- importは「他人が作った便利機能を使う宣言」と説明。
- fmtは出力用、と具体的に結びつける。
-->

## `import`

```go
import "fmt"
```

- 別のパッケージを使うために書く
- `fmt` は文字を表示するためによく使う
- 必要なものだけ `import` する

初心者ポイント:

- `import` したのに使わないとエラーになる

---

<!--
講師メモ:
- main関数 = 最初に動く場所。
- ここがエントリーポイントという言葉は補足程度でよい。
-->

## `func main()`

```go
func main() {
    fmt.Println("start")
}
```

- `func` は関数を作るキーワード
- `main()` はプログラム開始時に呼ばれる特別な関数
- 波かっこ `{}` の中に処理を書く

---

<!--
講師メモ:
- ドット記法に慣れていない学生には「fmtの中のPrintln」と説明。
- 行末のセミコロンが不要な点も軽く触れる。
-->

## `fmt.Println`

```go
fmt.Println("Hello")
fmt.Println(123)
fmt.Println(true)
```

- 画面に表示する関数
- `ln` は改行ありのイメージ
- 文字列、数値、真偽値をそのまま表示できる

---

<!--
講師メモ:
- ここで短い演習を入れて、まず実行の感覚を持たせる。
- 書き換えて動かすだけでよい。
-->

## ミニ演習 1

次のプログラムを書いて実行してみよう

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello")
    fmt.Println("Go")
}
```

できたら:

- 自分の名前を表示してみる

---

<!--
講師メモ:
- := と var の違いをまず見せる。
- 「最初は := をよく使う」で十分。
-->

## 変数

```go
name := "Taro"
age := 16

fmt.Println(name)
fmt.Println(age)
```

- 値を入れておく箱が変数
- `:=` は「変数を作って値を入れる」
- 関数の中でよく使う書き方

---

<!--
講師メモ:
- 型は「値の種類」。
- 推論されるので毎回書かなくてよい、がGoの書きやすさにつながる。
-->

## 型

よく使う型:

- `string` 文字列
- `int` 整数
- `bool` 真偽値

```go
var title string = "Go入門"
var score int = 80
var ok bool = true
```

- Goは型を大事にする言語

---

<!--
講師メモ:
- 型推論と明示の両方を見せる。
- 実務では読みやすさとのバランスで使うと補足してよい。
-->

## 型推論もできる

```go
title := "Go入門"
score := 80
ok := true

fmt.Println(title)
fmt.Println(score)
fmt.Println(ok)
```

- `:=` を使うと型を自動で決めてくれる
- 初学者はまずこちらに慣れるとよい

---

<!--
講師メモ:
- ゼロ値はGoらしい特徴。
- 「初期化していない変数にも最初の値がある」を強調。
-->

## ゼロ値

変数を作ると、最初から値が入る

```go
var name string
var age int
var ok bool

fmt.Println(name)
fmt.Println(age)
fmt.Println(ok)
```

- `string` は `""`
- `int` は `0`
- `bool` は `false`

---

<!--
講師メモ:
- constは「変わらない値」。
- 数学定数や設定値につながる話を軽くする。
-->

## 定数

```go
const school = "Kosen"
const taxRate = 0.1

fmt.Println(school)
fmt.Println(taxRate)
```

- `const` は変更しない値
- 後から書き換えられない
- 固定の設定値に向いている

---

<!--
講師メモ:
- ここまでの区切り。
- 変数と型で混乱しやすいので、小さい確認問題にする。
-->

## ミニ演習 2

1. 変数 `message` に `"Goはじめて"` を入れる
2. 変数 `count` に `3` を入れる
3. `fmt.Println` で両方表示する

書けたら挑戦:

- `const version = "1.0"` も追加する

---

<!--
講師メモ:
- ifは条件がtrueなら実行、と平易に。
- 括弧が不要な点は他言語との差分として触れる。
-->

## `if` 文

```go
score := 75

if score >= 60 {
    fmt.Println("pass")
}
```

- 条件が成り立つときだけ実行する
- Goでは `if (条件)` ではなく `if 条件`

---

<!--
講師メモ:
- elseも1枚で見る。
- 実務では入力チェックや認証分岐で使う、と軽くつなぐ。
-->

## `if` と `else`

```go
age := 17

if age >= 18 {
    fmt.Println("adult")
} else {
    fmt.Println("minor")
}
```

- 2つに分けたいときは `else`
- バックエンドでは条件分岐がとても多い

---

<!--
講師メモ:
- forはGoで重要。while相当も含めてここから数枚使う。
- まずは普通のループ。
-->

## `for` 文

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

- 繰り返し処理を書く
- `i := 0` は開始
- `i < 5` は続ける条件
- `i++` は1増やす

---

<!--
講師メモ:
- whileがないのはGoらしい特徴。
- ただし「forで書けるから困らない」と安心させる。
-->

## Goに`while`はない

でも、`for` で同じことができる

```go
count := 0

for count < 3 {
    fmt.Println(count)
    count++
}
```

- 条件だけ書くと、`while` のように使える

---

<!--
講師メモ:
- ここで制御構文の演習。
- 0から4、偶数奇数など簡単なものでよい。
-->

## ミニ演習 3

1. `for` を使って `1` から `5` を表示する
2. `if` を使って、数が偶数なら `"even"` を表示する

ヒント:

```go
if n%2 == 0 {
    fmt.Println("even")
}
```

---

<!--
講師メモ:
- 関数は「同じ処理に名前をつける」もの。
- main以外の関数を書けることを見せる。
-->

## 関数

```go
func greet() {
    fmt.Println("Hello")
}

func main() {
    greet()
}
```

- 処理をまとめて名前をつけられる
- 同じ処理を何回も使いやすくなる

---

<!--
講師メモ:
- 引数と戻り値を一緒に見せる。
- 数学の関数イメージで説明すると伝わりやすい。
-->

## 戻り値

```go
func add(a int, b int) int {
    return a + b
}

func main() {
    result := add(2, 3)
    fmt.Println(result)
}
```

- 入力: `a`, `b`
- 出力: `int`
- `return` で結果を返す

---

<!--
講師メモ:
- 複数戻り値はGoの重要文化だが深入りしない。
- エラー処理でよく使うとだけつなげる。
-->

## 複数戻り値の軽い紹介

```go
func divide(a int, b int) (int, bool) {
    if b == 0 {
        return 0, false
    }
    return a / b, true
}
```

- Goの関数は複数の値を返せる
- 実務では `結果, エラー` の形がよく出る

---

<!--
講師メモ:
- ここで総合演習に入る前の橋渡し。
- 「関数を分けるとCLIも読みやすい」と伝える。
-->

## ミニ演習 4

関数 `square` を作ろう

条件:

- 引数に `int` を1つ受け取る
- 2乗した結果を返す

例:

```go
fmt.Println(square(4)) // 16
```

---

<!--
講師メモ:
- ここが今日の到達点。
- 入力はまだ固定値でよい。CLIっぽく「表示する道具」を作る。
-->

## ミニCLIプログラム

偶数か奇数かを表示するプログラム

```go
package main

import "fmt"

func main() {
    number := 7

    if number%2 == 0 {
        fmt.Println("even")
    } else {
        fmt.Println("odd")
    }
}
```

---

<!--
講師メモ:
- 少しだけ実務に寄せて関数分割版を見せる。
- 長くしすぎず、mainが薄くなる良さを伝える。
-->

## 関数に分けると読みやすい

```go
package main

import "fmt"

func judge(n int) string {
    if n%2 == 0 {
        return "even"
    }
    return "odd"
}

func main() {
    fmt.Println(judge(7))
}
```

- バックエンドでも「小さく分ける」は大事

---

<!--
講師メモ:
- ありがちなエラーを先回りで見せる。
- 失敗しても普通だと伝える。
-->

## よくあるエラー

- `package main` を書き忘れる
- `import "fmt"` を忘れる
- 波かっこ `{}` の位置がずれる
- 使っていない変数を作る
- `:=` と `=` を混同する

例:

```go
name := "Taro"
name := "Jiro" // 同じ場所で再宣言してしまう
```

---

<!--
講師メモ:
- ここでは「今日できるようになったこと」を再確認。
- 次回以降への接続も軽く入れる。
-->

## まとめ

- Goはシンプルでバックエンド向きの言語
- `package main`、`import`、`func main()` が入口
- 変数、型、`if`、`for`、関数で基本的な処理が書ける
- 小さなCLIならもう読めるし書き始められる

---

<!--
講師メモ:
- 宿題は短く、成功体験が出やすいものにする。
- 次回につながるように、入力や標準ライブラリへの興味を残す。
-->

## 宿題

次のプログラムを作ってみよう

1. `name := "Taro"` を使ってあいさつを表示する
2. `score := 85` を使って、60点以上なら `"pass"`、それ以外は `"fail"` を表示する
3. 関数 `double(n int) int` を作って、2倍した結果を表示する

余裕があれば:

- 数字を3つ表示する `for` 文も入れてみる
