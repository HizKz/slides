---
marp: true
theme: default
paginate: true
size: 16:9
title: 第3回 struct・method・pointer
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
    font-size: 0.72em;
  }
  strong {
    color: #0f766e;
  }
  table {
    font-size: 0.88em;
  }
  section.cli-code pre code {
    font-size: 0.62em;
  }
---

<!--
講師メモ:
- 最後にユーザー管理CLIを完成させる回だと先に示す。
- pointerは「値を直接変更するための道具」から始める。
-->

# 第3回: struct・method・pointer

Go初心者向け / 高専生向け

- データをひとまとめにする
- データに処理を持たせる
- 元の値を直接変更する

---

<!--
講師メモ:
- 3分程度。sliceとmapを口頭で説明してもらう。
-->

## 前回の復習

第2回で学んだこと:

- 関数で処理を分ける
- `slice` で複数の値を持つ
- `map` でキーと値を対応させる
- `range` で一覧を繰り返す

```go
names := []string{"Alice", "Bob"}
scores := map[string]int{"Alice": 90}
```

今日は、**1人分のデータ**をきれいにまとめる。

---

<!--
講師メモ:
- ゴールは「説明できる」ではなく「書ける」で確認する。
-->

## 今日のゴール

- `struct` で関連するデータをまとめられる
- `method` でデータ専用の処理を書ける
- `pointer` の基本を理解する
- 値渡しとポインタ渡しを使い分けられる
- 簡単なユーザー管理CLIを作れる

---

<!--
講師メモ:
- structは「自分で作る新しい型」と説明する。
-->

## structとは

**複数の関連する値を、1つにまとめる型**

```go
type User struct {
    Name string
    Age  int
}
```

`User` は次の2つを持つ:

- 名前 `Name`
- 年齢 `Age`

---

<!--
講師メモ:
- 複数slice方式ではデータの対応が崩れやすいと示す。
-->

## structを使う理由

別々に持つと、対応関係が分かりにくい:

```go
names := []string{"Alice", "Bob"}
ages := []int{18, 19}
```

`struct` なら1人分が1つになる:

```go
alice := User{Name: "Alice", Age: 18}
```

**データの意味とまとまりがコードに表れる。**

---

<!--
講師メモ:
- type、型名、struct、フィールドの順に指差して読む。
-->

## structの定義

```go
package main

import "fmt"

type User struct {
    Name string
    Age  int
}

func main() {
    fmt.Println(User{})
}
```

- `type User`: `User` という型を作る
- `Name`, `Age`: フィールド
- フィールドごとに型を決める

---

<!--
講師メモ:
- フィールド名付きの初期化を基本として勧める。
-->

## structの生成

```go
package main

import "fmt"

type User struct {
    Name string
    Age  int
}

func main() {
    user := User{Name: "Alice", Age: 18}
    fmt.Println(user)
}
```

`型名{フィールド名: 値}` で生成する。

---

<!--
講師メモ:
- ドットは「userのName」と読ませる。
-->

## フィールドへのアクセス

```go
package main

import "fmt"

type User struct {
    Name string
    Age  int
}

func main() {
    user := User{Name: "Alice", Age: 18}
    fmt.Println(user.Name)

    user.Age = 19
    fmt.Println(user.Age)
}
```

`.` でフィールドを読み書きできる。

---

<!--
講師メモ:
- ゼロ値は未入力と同じ意味とは限らない点には深入りしない。
-->

## structのゼロ値

```go
package main

import "fmt"

type User struct {
    Name   string
    Age    int
    Active bool
}

func main() {
    var user User
    fmt.Printf("%q %d %t\n", user.Name, user.Age, user.Active)
}
```

初期値は各型のゼロ値: `""`, `0`, `false`

---

<!--
講師メモ:
- 5分。まず紙やコメントでフィールドを考えさせる。
-->

## ミニ演習: User構造体を作る

次のフィールドを持つ `User` を作ろう:

| フィールド | 型 | 例 |
| --- | --- | --- |
| `ID` | `int` | `1` |
| `Name` | `string` | `"Taro"` |
| `Email` | `string` | `"taro@example.com"` |

1. `User` 型を定義する
2. 1人生成する
3. 名前とメールアドレスを表示する

---

<!--
講師メモ:
- 前回のsliceと今日のstructを接続する重要なスライド。
-->

## structのslice

```go
package main

import "fmt"

type User struct{ Name string }

func main() {
    users := []User{
        {Name: "Alice"},
        {Name: "Bob"},
    }
    for _, user := range users {
        fmt.Println(user.Name)
    }
}
```

一覧は `[]User` で表せる。

---

<!--
講師メモ:
- ID検索の形は、DBやキャッシュにつながると説明する。
-->

## structとmapの組み合わせ

```go
package main

import "fmt"

type User struct{ Name string }

func main() {
    users := map[int]User{
        1: {Name: "Alice"},
        2: {Name: "Bob"},
    }

    user, ok := users[2]
    fmt.Println(user.Name, ok)
}
```

`map[int]User` は **IDからユーザーを探す**形。

---

<!--
講師メモ:
- JSONタグは読めれば十分。encoding/jsonの詳細は扱わない。
-->

## バックエンドでのstructの使い道

```go
type CreateUserRequest struct {
    Name  string `json:"name"`
    Email string `json:"email"`
}

type UserResponse struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
}

type UserModel struct {
    ID    int
    Email string
}
```

- APIの **Request / Response**
- DBの1行を表す **Model**
- 設定値や処理結果

---

<!--
講師メモ:
- struct章の確認。答えは次の章に持ち越さない。
-->

## ミニ演習: ユーザー一覧

1. 先ほどの `User` を3人作る
2. `[]User` に入れる
3. `range` で `ID: Name` を表示する

完成イメージ:

```text
1: Taro
2: Hanako
3: Sora
```

追加課題: `map[int]User` に入れてIDから検索する。

---

<!--
講師メモ:
- methodは「特定の型に結びついた関数」と説明する。
-->

## methodとは

`User` 専用の処理を `User` に結びつけられる。

```go
func (u User) Greet() {
    fmt.Println("こんにちは、" + u.Name + "です")
}
```

呼び出し方:

```go
user.Greet()
```

**データと、そのデータに関係する処理**を近くに置ける。

---

<!--
講師メモ:
- receiver部分を隠さず、一つずつ読む。
-->

## methodの基本形

```go
package main

import "fmt"

type User struct{ Name string }

func (u User) Greet() {
    fmt.Println("こんにちは、" + u.Name + "です")
}

func main() {
    user := User{Name: "Alice"}
    user.Greet()
}
```

`func (receiver) Method名()` が基本形。

---

<!--
講師メモ:
- receiver名は短く型の頭文字が慣例。this/selfではない。
-->

## receiverとは

```go
func (u User) Greet() {
    fmt.Println(u.Name)
}
```

`(u User)` が receiver:

- methodを呼び出した値を受け取る
- method内では `u` で使う
- `User` の頭文字で `u` とすることが多い

```go
user.Greet() // userがuに渡る
```

---

<!--
講師メモ:
- 値receiverはコピー。表示や計算に向くと押さえる。
-->

## 値receiver

```go
package main

import "fmt"

type User struct{ Name string }

func (u User) Rename(name string) {
    u.Name = name
}

func main() {
    user := User{Name: "Alice"}
    user.Rename("Bob")
    fmt.Println(user.Name) // Alice
}
```

`u` はコピーなので、元の `user` は変わらない。

---

<!--
講師メモ:
- *Userの意味は後半で詳しく扱う。ここでは結果の違いを見る。
-->

## pointer receiver

```go
package main

import "fmt"

type User struct{ Name string }

func (u *User) Rename(name string) {
    u.Name = name
}

func main() {
    user := User{Name: "Alice"}
    user.Rename("Bob")
    fmt.Println(user.Name) // Bob
}
```

`*User` なら、元の `user` を変更できる。

---

<!--
講師メモ:
- 呼び出しの読みやすさに注目させる。
-->

## methodと関数の違い

```go
func Greet(u User) {
    fmt.Println(u.Name)
}

func (u User) Greet() {
    fmt.Println(u.Name)
}
```

| 関数 | method |
| --- | --- |
| `Greet(user)` | `user.Greet()` |
| どの型にも所属しない | `User` に結びつく |

`User` に強く関係する処理なら method が自然。

---

<!--
講師メモ:
- 5分。値receiverでよい理由も質問する。
-->

## ミニ演習: 自己紹介method

`User` に `Introduce` methodを追加しよう。

```go
user := User{Name: "Taro", Age: 18}
user.Introduce()
```

期待する出力:

```text
Taroです。18歳です。
```

ヒント:

```go
func (u User) Introduce() { /* ... */ }
```

---

<!--
講師メモ:
- メモリ番地の詳細には進まず、住所という比喩を短く使う。
-->

## pointerとは

pointerは、**値が置かれている場所を指す値**。

```text
user       -> User{Name: "Alice"}
&user      -> userが置かれている場所
```

まず覚える用途:

- 関数やmethodから元の値を変更したい
- 大きな値を毎回コピーしたくない

今日は特に **元の値を直接変更する**ことに注目する。

---

<!--
講師メモ:
- &で取得、*で参照の2つだけを動かして確認する。
-->

## `&` と `*`

```go
package main

import "fmt"

func main() {
    number := 10
    p := &number

    fmt.Println(*p) // 10
    *p = 20
    fmt.Println(number) // 20
}
```

- `&number`: `number` の場所を取得
- `*p`: その場所にある値を読み書き

---

<!--
講師メモ:
- 実行前に出力を予想させる。
-->

## 値渡し

```go
package main

import "fmt"

func change(n int) {
    n = 100
}

func main() {
    number := 10
    change(number)
    fmt.Println(number) // 10
}
```

関数に値の**コピー**が渡る。元の値は変わらない。

---

<!--
講師メモ:
- &numberと*nを対応させる。
-->

## ポインタ渡し

```go
package main

import "fmt"

func change(n *int) {
    *n = 100
}

func main() {
    number := 10
    change(&number)
    fmt.Println(number) // 100
}
```

場所を渡し、その場所にある値を変更する。

---

<!--
講師メモ:
- 迷ったら「変更するmethodか」で判断する基準を渡す。
-->

## なぜpointerを使うのか

主な理由は2つ:

1. **元の値を変更するため**
2. 大きなstructのコピーを避けるため

今回の判断基準:

```go
func (u User) DisplayName() string // 読むだけ
func (u *User) Rename(name string) // 変更する
```

迷ったら、変更するmethodには pointer receiver。

---

<!--
講師メモ:
- nil dereferenceの詳細やrecoverには進まない。
-->

## nilの軽い紹介

pointerは「どこも指していない」ことがある。

```go
package main

import "fmt"

func main() {
    var p *int

    if p == nil {
        fmt.Println("値を指していません")
    }
}
```

- `nil` のpointerを `*p` で読むと実行時エラー
- 使う前に `nil` か確認する場面がある

---

<!--
講師メモ:
- &と*の位置を見比べる。コンパイルエラーも学習材料にする。
-->

## よくあるつまずき: 記号

```go
number := 10
p := &number // 場所を得る
*p = 20     // その場所の値を変える
```

混同しやすい点:

- `*int`: intを指すpointerの**型**
- `*p`: pが指す場所の**値**
- `&number`: numberの**場所**

記号だけでなく、式全体を声に出して読む。

---

<!--
講師メモ:
- Goが自動で&を補うため、method呼び出しでは&が見えないと説明する。
-->

## よくあるつまずき: receiver

```go
func (u User) Rename(name string) {  // コピー
    u.Name = name
}

func (u *User) Rename(name string) { // 元を変更
    u.Name = name
}
```

注意:

- 同じ型に同名methodを2つは定義できない
- 変更したいなら `*User`
- `user.Rename(...)` の呼び出し方は同じ

---

<!--
講師メモ:
- 5分。値渡し版の出力を予想してからpointer版を書く。
-->

## ミニ演習: 年齢を1つ増やす

`Birthday` methodで元の年齢を増やそう。

```go
package main

import "fmt"

type User struct{ Age int }

func (u *User) Birthday() {
    // ここを書く
}

func main() {
    user := User{Age: 18}
    user.Birthday()
    fmt.Println(user.Age) // 19
}
```

---

<!--
講師メモ:
- ここから15〜20分。完成形を小さな部品に分けて組み立てる。
-->

## 実践: 簡単なユーザー管理CLI

作る機能:

- ユーザーを追加する
- 登録済みユーザーを一覧表示する
- IDを指定して名前を変更する

使うもの:

- `User` struct
- `[]User`
- 値receiver / pointer receiver
- 関数、`range`

---

<!--
講師メモ:
- まずデータの設計だけを書く。Activeも表示に利用する。
-->

## CLI: Userを定義する

```go
type User struct {
    ID     int
    Name   string
    Active bool
}

func (u User) Display() {
    status := "停止中"
    if u.Active {
        status = "有効"
    }
    fmt.Printf("%d: %s [%s]\n", u.ID, u.Name, status)
}
```

表示だけなので値receiverを使う。

---

<!--
講師メモ:
- 名前変更は元のUserを変えるためpointer receiver。
-->

## CLI: 名前を変更する

```go
func (u *User) Rename(name string) {
    u.Name = name
}
```

呼び出した後も変更が残る:

```go
user := User{ID: 1, Name: "Taro", Active: true}
user.Rename("Sora")
user.Display()
```

```text
1: Sora [有効]
```

---

<!--
講師メモ:
- slice自体を増やすため、追加関数は新しいsliceを返す形にする。
-->

## CLI: ユーザーを追加する

```go
func addUser(users []User, id int, name string) []User {
    user := User{
        ID:     id,
        Name:   name,
        Active: true,
    }
    return append(users, user)
}
```

`append` 後のsliceを戻り値で受け取る:

```go
users = addUser(users, 1, "Taro")
```

---

<!--
講師メモ:
- indexでアクセスする理由を強調。rangeのuserを直接変更する話は発展扱い。
-->

## CLI: IDで探して変更する

```go
func renameByID(users []User, id int, name string) bool {
    for i := range users {
        if users[i].ID == id {
            users[i].Rename(name)
            return true
        }
    }
    return false
}
```

- 見つけたら名前を変更して `true`
- 見つからなければ `false`

---

<!--
講師メモ:
- このコードはそのままmain.goとして動く。順に追って実行する。
-->

## CLI: 完成コード 1/2

```go
package main

import "fmt"

type User struct {
    ID     int
    Name   string
    Active bool
}

func (u User) Display() {
    status := "停止中"
    if u.Active {
        status = "有効"
    }
    fmt.Printf("%d: %s [%s]\n", u.ID, u.Name, status)
}

func (u *User) Rename(name string) {
    u.Name = name
}
```

---

<!--
講師メモ:
- add、一覧、更新の順に動作を確認する。
-->

## CLI: 完成コード 2/2

<!-- _class: cli-code -->

```go
func addUser(users []User, id int, name string) []User {
    return append(users, User{ID: id, Name: name, Active: true})
}

func renameByID(users []User, id int, name string) bool {
    for i := range users {
        if users[i].ID == id {
            users[i].Rename(name)
            return true
        }
    }
    return false
}

func main() {
    users := addUser(nil, 1, "Taro")
    users = addUser(users, 2, "Hanako")
    if !renameByID(users, 2, "Sora") {
        fmt.Println("ユーザーが見つかりません")
    }
    for _, user := range users {
        user.Display()
    }
}
```

---

<!--
講師メモ:
- 余裕があるクラス向け。入力受付は次の発展につながる。
-->

## CLI: 次に追加できる機能

- `bufio.Scanner` で名前を入力する
- 同じIDを登録できないようにする
- ユーザーを停止する `Deactivate` method
- IDでユーザーを検索する
- ファイルやDBへ保存する

バックエンドでも基本の流れは同じ:

```text
入力 -> structにまとめる -> 処理する -> 保存・出力
```

---

<!--
講師メモ:
- 生徒に各項目の短い説明をしてもらう。
-->

## まとめ

- `struct`: 関連するデータを1つの型にまとめる
- `method`: 型に関係する処理を結びつける
- 値receiver: コピーを使い、元の値を変えない
- pointer receiver: 元の値を変更できる
- `&`: 値の場所を得る
- `*`: pointerが指す値を使う
- `[]User` で複数ユーザーを管理できる

---

<!--
講師メモ:
- 必須要件を一つずつチェックさせる。
- 余裕があれば商品名も変更できるmethodを追加する。
-->

## 宿題: 商品管理プログラム

次の要件をすべて実装しよう:

1. `Name string`, `Price int` を持つ `Product` を作る
2. 税込価格を返す `TaxIncludedPrice` methodを作る
3. `[]Product` に商品を3つ以上入れる
4. 税抜価格が1000円以上の商品だけ表示する
5. pointer receiverで価格を変更する `ChangePrice` methodを作る

税込価格は簡単のため次で計算してよい:

```go
return p.Price * 110 / 100
```
