---
marp: true
theme: default
paginate: true
size: 16:9
title: 第2回 関数・slice・map
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
    font-size: 0.88em;
  }
  pre code {
    font-size: 0.74em;
  }
  strong {
    color: #0f766e;
  }
---

<!--
講師メモ:
- 第1回より「実務でどう使うか」を少し意識させる。
- 関数で分ける -> データをまとめる -> 集計する、という流れで通す。
-->

# 第2回: 関数・slice・map

Go初心者向け  
高専生向け入門

- 処理を分けて書く
- 複数データをまとめて扱う
- 最後に簡単な集計CLIを作る

---

<!--
講師メモ:
- 前回の内容を2〜3分で確認する。
- 「mainの中に全部書けるが、今日はそこから一歩進む」とつなげる。
-->

## 前回の復習

- `package main` と `func main()` を学んだ
- 変数、型、`if`、`for` を使った
- 関数の基本を少し見た

今日からの一歩:

- **複数の値をまとめて処理する**
- **読みやすい形に分けて書く**

---

<!--
講師メモ:
- ゴールは行動で言う。
- 最後のCLIが着地点だと先に見せておく。
-->

## 今日のゴール

- 関数を使って処理を分けられる
- `slice` の基本操作を理解する
- `map` でキーと値のデータを扱える
- `range` で繰り返し処理できる
- 簡単な集計CLIを書ける

---

<!--
講師メモ:
- 「mainに全部書くとつらい」を実感させる。
- バックエンドでは関数分割が重要と触れる。
-->

## 関数をなぜ使うのか

処理を分けると:

- 読みやすい
- 再利用しやすい
- テストしやすい
- バグの場所を見つけやすい

バックエンドとのつながり:

- APIで受け取ったデータの整形
- DB結果の集計
- 料金計算や入力チェック

---

<!--
講師メモ:
- まず一番基本の形だけ。
- 呼び出し側と定義側を見比べる。
-->

## 関数の基本形

```go
package main

import "fmt"

func greet() {
    fmt.Println("こんにちは")
}

func main() {
    greet()
}
```

- `func` で関数を作る
- `greet()` で呼び出す

---

<!--
講師メモ:
- 引数は「関数に渡す入力」と説明する。
-->

## 引数

```go
package main

import "fmt"

func greet(name string) {
    fmt.Println("こんにちは,", name)
}

func main() {
    greet("Taro")
}
```

- `name string` が引数
- 関数ごとに必要な情報を渡せる

---

<!--
講師メモ:
- 戻り値があると「計算して返す」関数になる。
- printする関数との違いを意識させる。
-->

## 戻り値

```go
package main

import "fmt"

func add(a int, b int) int {
    return a + b
}

func main() {
    total := add(3, 4)
    fmt.Println(total)
}
```

- `int` が戻り値の型
- `return` で結果を返す

---

<!--
講師メモ:
- Goらしい特徴として軽く強調。
- エラー処理につながると後で役立つ。
-->

## 複数戻り値

```go
package main

import "fmt"

func divMod(a int, b int) (int, int) {
    return a / b, a % b
}

func main() {
    q, r := divMod(10, 3)
    fmt.Println(q, r)
}
```

- 1つの関数から複数の値を返せる
- Goでは **結果 + エラー** を返す形でもよく使う

---

<!--
講師メモ:
- 名前付き戻り値は深追いしない。
- 読めれば十分、基本は通常の return を勧める。
-->

## 名前付き戻り値

```go
func calc(a int, b int) (sum int) {
    sum = a + b
    return
}
```

- 戻り値に名前をつける書き方
- 便利な場面もある
- **初心者のうちは普通の `return 値` を主に使えばOK**

---

<!--
講師メモ:
- 「関数に切り出すとmainが短くなる」を見せる。
-->

## 関数で分けると何がうれしいか

```go
func calcTaxIncluded(price int) int {
    return int(float64(price) * 1.10)
}
```

`main` 側はこう書ける:

```go
price := 1200
fmt.Println(calcTaxIncluded(price))
```

- 計算の意味が名前で伝わる
- 同じ計算を何度でも使える

---

<!--
講師メモ:
- ここは3〜5分の短い演習。
- 税率を変えてみる追加課題も入れられる。
-->

## ミニ演習: 税込価格を計算する関数

やること:

1. `calcTaxIncluded(price int) int` を作る
2. 税抜き `100`, `500`, `980` を試す

完成イメージ:

```go
fmt.Println(calcTaxIncluded(100))
fmt.Println(calcTaxIncluded(500))
fmt.Println(calcTaxIncluded(980))
```

---

<!--
講師メモ:
- sliceの前に配列を少しだけ入れる。
- 「固定長」とだけ押さえれば十分。
-->

## 配列とは

```go
package main

import "fmt"

func main() {
    scores := [3]int{80, 90, 75}
    fmt.Println(scores)
}
```

- 配列は **長さが固定**
- `[3]int` は「`int` を3個」
- Goでは後で学ぶ `slice` の土台になる

---

<!--
講師メモ:
- 実際によく使うのはsliceだとはっきり言う。
-->

## sliceとは

```go
package main

import "fmt"

func main() {
    scores := []int{80, 90, 75}
    fmt.Println(scores)
}
```

- `slice` は **長さを増やせる**
- 複数の値をまとめて扱える
- Goでとてもよく使う

実務では:

- APIの一覧データ
- DB検索結果の複数行

---

<!--
講師メモ:
- 配列とsliceの違いを一度整理する。
-->

## 配列とsliceの違い

| 項目 | 配列 | slice |
| --- | --- | --- |
| 長さ | 固定 | 変えられる |
| 書き方 | `[3]int` | `[]int` |
| 使う場面 | 少なめ | 多い |

結論:

- 初心者はまず **`slice` を主役** と考えてよい

---

<!--
講師メモ:
- リテラルとmakeの2通りを見せる。
-->

## sliceの作り方

```go
numbers := []int{1, 2, 3}
```

```go
names := make([]string, 3)
```

- `[]int{...}` は最初から値を入れる
- `make([]string, 3)` は長さ3のsliceを作る

```go
fmt.Println(names) // [  ]
```

---

<!--
講師メモ:
- appendはかなり重要。毎回戻り値を受けることを強調。
-->

## `append`

```go
package main

import "fmt"

func main() {
    nums := []int{1, 2}
    nums = append(nums, 3)
    nums = append(nums, 4, 5)
    fmt.Println(nums)
}
```

- 要素を後ろに追加する
- **`append` の結果を代入し直す**

---

<!--
講師メモ:
- lenは必須、capは雰囲気だけでよい。
-->

## `len` と `cap`

```go
package main

import "fmt"

func main() {
    nums := make([]int, 2, 5)
    fmt.Println(len(nums)) // 2
    fmt.Println(cap(nums)) // 5
}
```

- `len`: 今入っている要素数
- `cap`: 余裕を含めた容量

初心者向けの覚え方:

- まずは **`len` が重要**

---

<!--
講師メモ:
- indexとvalueの2つが取れることを見せる。
- `_` も軽く紹介。
-->

## `range` でsliceを回す

```go
package main

import "fmt"

func main() {
    scores := []int{80, 90, 75}

    for i, score := range scores {
        fmt.Println(i, score)
    }
}
```

- `i` は添字
- `score` は要素
- どちらか不要なら `_` を使う

---

<!--
講師メモ:
- 平均はint除算になる点を軽く扱う。
- 今回は小数までこだわらず int 平均でもよい。
-->

## slice + `range` の定番パターン

```go
func sumScores(scores []int) int {
    total := 0
    for _, score := range scores {
        total += score
    }
    return total
}
```

- `range` は一覧データ処理の基本
- APIレスポンスの配列処理でも同じ形を使う

---

<!--
講師メモ:
- ここで関数とsliceをつなぐ。
-->

## ミニ演習: 点数一覧の平均を出す

やること:

1. `scores := []int{80, 90, 75, 95}`
2. 合計を出す関数を作る
3. `len(scores)` で平均を出す

ヒント:

```go
average := total / len(scores)
```

---

<!--
講師メモ:
- mapは「名前で引く箱」と説明。
- sliceとの違いを明確にする。
-->

## mapとは

```go
package main

import "fmt"

func main() {
    scores := map[string]int{
        "Alice": 90,
        "Bob":   78,
    }
    fmt.Println(scores)
}
```

- `キー -> 値` の対応を持つ
- 順番ではなく **名前で取り出す**

実務では:

- JSONの項目
- IDごとのデータ管理

---

<!--
講師メモ:
- makeでも作れることを教える。
-->

## mapの作り方

```go
prices := map[string]int{
    "apple":  120,
    "banana": 150,
}
```

```go
counts := make(map[string]int)
```

- 最初から値を入れる方法
- 空のmapを作る方法

---

<!--
講師メモ:
- 追加と更新は同じ書き方だと示す。
-->

## mapへの追加・更新

```go
package main

import "fmt"

func main() {
    scores := make(map[string]int)
    scores["Alice"] = 90
    scores["Bob"] = 78
    scores["Alice"] = 95
    fmt.Println(scores)
}
```

- なければ追加
- あれば更新

---

<!--
講師メモ:
- 存在しないキーのときの挙動を次のslideにつなぐ。
-->

## mapから値を取り出す

```go
package main

import "fmt"

func main() {
    prices := map[string]int{"apple": 120}
    fmt.Println(prices["apple"])
}
```

- `map[キー]` で値を取り出す
- ただし「そのキーが本当にあるか」は別問題

---

<!--
講師メモ:
- Goらしい書き方として丁寧に。
- APIの項目有無チェックに使うとつなげる。
-->

## comma ok idiom

```go
package main

import "fmt"

func main() {
    prices := map[string]int{"apple": 120}

    price, ok := prices["banana"]
    fmt.Println(price, ok)
}
```

- `price` は値
- `ok` はそのキーが存在したか
- 項目の有無を安全に確認できる

---

<!--
講師メモ:
- ifと合わせて実用形を見せる。
-->

## `comma ok` のよくある形

```go
if price, ok := prices["apple"]; ok {
    fmt.Println("価格:", price)
} else {
    fmt.Println("商品がありません")
}
```

- 1行で取得と確認ができる
- JSONや設定値のチェックでもよく使う

---

<!--
講師メモ:
- deleteは静かに消える、存在しなくてもエラーではない。
-->

## `delete`

```go
package main

import "fmt"

func main() {
    scores := map[string]int{"Alice": 90, "Bob": 78}
    delete(scores, "Bob")
    fmt.Println(scores)
}
```

- `delete(map, key)` で削除
- そのキーがなくてもエラーにはならない

---

<!--
講師メモ:
- mapは順番が安定しない点を必ず伝える。
-->

## `range` でmapを回す

```go
package main

import "fmt"

func main() {
    scores := map[string]int{"Alice": 90, "Bob": 78}

    for name, score := range scores {
        fmt.Println(name, score)
    }
}
```

- キーと値を順に取り出せる
- **表示順は毎回同じとは限らない**

---

<!--
講師メモ:
- mapの繰り返しに慣れさせる。
-->

## ミニ演習: 名前ごとの点数を表示する

やること:

1. `map[string]int` で点数表を作る
2. `range` で全員ぶん表示する

例:

```go
scores := map[string]int{
    "Alice": 90,
    "Bob":   78,
    "Carol": 88,
}
```

---

<!--
講師メモ:
- 最後の実践に入る前に要件を簡単に整理する。
- 関数, slice, map, range が全部出てくると示す。
-->

## 実践: 簡単な集計CLI

作るもの:

- 商品名ごとの売上個数を持つ
- 合計個数を出す
- しきい値以上の商品だけ表示する

使う要素:

- 関数
- `map`
- `range`
- `if`

---

<!--
講師メモ:
- まずは全体像を先に見せる。
- コードをいきなり全部読む必要はないと伝える。
-->

## 完成イメージ

```text
売上個数:
apple 12
banana 7
orange 15
合計: 34
10個以上の商品:
apple
orange
```

- CLIは「ターミナルで動く小さなツール」
- バックエンド周辺の運用でもよく使う

---

<!--
講師メモ:
- まずデータだけ置く。
- mapで「商品名 -> 個数」を表す。
-->

## CLI: データを用意する

```go
sales := map[string]int{
    "apple":  12,
    "banana": 7,
    "orange": 15,
}
```

- 商品名をキーにする
- 個数を値にする

DB結果やAPI結果も、集計前にこうした形で持つことが多い

---

<!--
講師メモ:
- 集計関数に切り出す。
-->

## CLI: 合計を計算する関数

```go
func totalCount(sales map[string]int) int {
    total := 0
    for _, count := range sales {
        total += count
    }
    return total
}
```

- `range` で値を集める
- 関数にすると再利用しやすい

---

<!--
講師メモ:
- 条件付き表示で if を再確認。
-->

## CLI: 条件に合う商品だけ表示する

```go
func printLargeSales(sales map[string]int, min int) {
    for name, count := range sales {
        if count >= min {
            fmt.Println(name)
        }
    }
}
```

- `min` が条件
- 集計やフィルタはバックエンドで頻出

---

<!--
講師メモ:
- 完成版は短く、ちゃんと動く形にする。
- 実際に打って動かせる長さに抑える。
-->

## CLI: 完成コード

```go
package main

import "fmt"

func totalCount(sales map[string]int) int {
    total := 0
    for _, count := range sales {
        total += count
    }
    return total
}

func printLargeSales(sales map[string]int, min int) {
    for name, count := range sales {
        if count >= min {
            fmt.Println(name)
        }
    }
}

func main() {
    sales := map[string]int{
        "apple":  12,
        "banana": 7,
        "orange": 15,
    }

    fmt.Println("売上個数:")
    for name, count := range sales {
        fmt.Println(name, count)
    }

    fmt.Println("合計:", totalCount(sales))
    fmt.Println("10個以上の商品:")
    printLargeSales(sales, 10)
}
```

---

<!--
講師メモ:
- 初心者が詰まりやすい点を具体的に。
-->

## よくあるエラー 1

```go
nums := []int{1, 2}
append(nums, 3)
```

これはダメ:

- `append` の結果を受け取っていない

正しくは:

```go
nums = append(nums, 3)
```

---

<!--
講師メモ:
- mapのゼロ値と存在確認をセットで。
-->

## よくあるエラー 2

```go
price := prices["banana"]
```

これだけだと:

- 0が「本当に入っていた値」か
- 「キーがなかった」のか

区別しにくい

正しくは:

```go
price, ok := prices["banana"]
```

---

<!--
講師メモ:
- rangeの対象ごとの違いを整理する。
-->

## よくあるエラー 3

- `range` できる対象を混同する

```go
for i, v := range scores {   // slice
    fmt.Println(i, v)
}

for k, v := range prices {   // map
    fmt.Println(k, v)
}
```

- `slice`: 添字 + 要素
- `map`: キー + 値

---

<!--
講師メモ:
- 最後は「できるようになったこと」を言語化する。
-->

## まとめ

- 関数で処理を分けられるようになった
- `slice` で複数データを扱えるようになった
- `map` でキーと値の対応を扱えるようになった
- `range` で一覧データを回せるようになった
- 簡単な集計CLIの形を作れた

---

<!--
講師メモ:
- 宿題は今日の総復習になるようにする。
- 余裕があれば関数を2つに分ける追加課題を出す。
-->

## 宿題

次のプログラムを作ってみよう

1. 商品名と価格の `map` を作る
2. 全商品の合計金額を計算する関数を書く
3. `1000` 円以上の商品だけ表示する

例:

```go
items := map[string]int{
    "keyboard": 2980,
    "mouse":    1500,
    "pen":      120,
}
```

次回につながる視点:

- データが増えても回せる形にしておく
