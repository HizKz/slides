---
marp: true
theme: mint
size: 16:9
paginate: true
---

<!-- _class: cover -->

<img src="images/gopher.png" alt="Goのマスコット Gopher" style="position:absolute; right:120px; top:120px; height:410px;">

# Goって<br>何がそんなにいいの？

APIサーバーの開発・運用から考える

<span style="position:absolute; left:104px; bottom:35px; font-size:15px; color:#536d62;">Gopher: Renee French / Takuya Ueda · <a href="https://github.com/golang-samples/gopher-vector">出典</a> · <a href="https://creativecommons.org/licenses/by/3.0/">CC BY 3.0</a></span>

<!--
約30秒：Goを初めて検討するエンジニアに向けて、APIサーバーを例に開発から運用までを見る。言語の機能一覧ではなく、どんな制約の下で何がうれしいのかを考える。
-->

---

# Go言語ってなんなの？

**Googleで生まれた、オープンソースのプログラミング言語**

- 2007年に開発が始まり、2009年に公開
- 静的型付け・コンパイル型で、並行処理の仕組みを備える
- APIサーバーやCLI、クラウドの基盤などで使われる

大規模な開発やネットワーク処理を、扱いやすくすることを目指した。

<span class="small">出典：[Go公式 FAQ](https://go.dev/doc/faq) ／ [Go for Cloud](https://go.dev/solutions/cloud)</span>

<!--
約40秒：Goは2007年にGoogleで設計が始まり、2009年にオープンソースとして公開された汎用言語。静的型付けとコンパイル、ガベージコレクション、並行処理が特徴。ネットワークや複数人での開発を扱いやすくすることが設計の動機にある。現在はGoogleだけでなく、世界中のコミュニティが開発に参加している。
-->

---

# Goを選ぶと何が変わる？

APIサーバーの開発を、次の3つで見てみる。

- **書く・読む**：チームでコードの認知負荷を抑える
- **処理をつなぐ**：並行処理とキャンセルを扱う
- **運用する**：ビルド、配布、観測を同じ道具立てで進める

<span class="small">参考：[Go for Cloud & Network Services](https://go.dev/solutions/cloud)</span>

<!--
約40秒：Goが絶対に優れているという話ではない。複数人で長く運用するAPIサーバーを例に、言語とツールの組み合わせが何に効くかを考える。
-->

---

# 他言語との比較：開発と実行の形

| 言語 | 型と主な実行方法 | 並行処理の代表例 | 実務での着眼点 |
|:--|:--|:--|:--|
| **Go** | 静的型・ネイティブ実行 | goroutine / channel | 道具と書き方が比較的揃う |
| TypeScript | 静的検査・JSで実行 | async / await | フロントと型・知識を共有 |
| Java | 静的型・主にJVM | 仮想スレッドなど | 成熟したライブラリと運用基盤 |
| C# | 静的型・主に.NET | Task / async・await | .NETの統合された開発環境 |
| Python | 動的型・型ヒント可 | asyncioなど | 記述量と豊富な周辺ライブラリ |

<span class="small">代表例であり、実行・配布方法には別の選択肢もあります。<br>参考：[Go](https://go.dev/doc/faq) ／ [TS](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch) ／ [Java](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html) ／ [C#](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/) ／ [Python](https://docs.python.org/3/library/typing.html)</span>

<!--
約55秒：各行の着眼点は一般的な傾向で、優劣の順位ではない。TypeScriptはJSに変換されNode.jsなどで動き、JavaとC#にもネイティブ実行の選択肢がある。Pythonの型ヒントは通常ランタイムでは強制されない。Javaの仮想スレッドもI/O待ちの多いサーバーで有力。Goだけが並行処理できるわけではない。
-->

---

# Goの文化：読み手のための一貫性

- `gofmt` が書式を揃え、レビューで議論すべき差を減らす
- `error` を戻り値で扱い、失敗の分岐をコード上に見せる
- 小さなインターフェースを使い、必要な振る舞いを表す

**一貫した道具は出発点。設計や命名の質はレビューで磨く。**

<span class="small">参考：[Effective Go](https://go.dev/doc/effective_go) ／ [Code Review Comments](https://go.dev/wiki/CodeReviewComments)</span>

<!--
約45秒：idiomatic Goは、技術的に書けることだけではなく、読み手に分かりやすい形を重視する慣習。gofmtで設計まで揃うわけではない。error処理の明示は見通しに役立つ一方、繰り返しが増えると感じる場面もある。
-->

---

# エラー処理：例外と戻り値

<div class="columns">
<div>

## TypeScriptの例

```ts
try {
  const user = await findUser(id)
  return user
} catch (err) {
  throw new Error("load user", { cause: err })
}
```

</div>
<div>

## Goの例

```go
user, err := findUser(ctx, id)
if err != nil {
    return User{}, fmt.Errorf(
        "load user: %w", err,
    )
}
return user, nil
```

</div>
</div>

<span class="small">Goでは通常のエラーを戻り値で返す。例は抜粋です。<br>参考：[Working with Errors in Go](https://go.dev/blog/go1.13-errors)</span>

<!--
約50秒：TypeScriptでは例外をcatchして原因付きの新しい例外を投げ直す例。Goは結果とerrorを返し、失敗を確認して文脈を加えて返す。どちらも言語や実装方法による選択肢があり、例外は暗黙、戻り値は必ず安全、と単純化しない。Goにもpanic/recoverがあり、通常のエラー処理とは用途を区別する。
-->

---

# `if err != nil` のよさ

- **見通し**：失敗を扱う位置が呼び出し元で見える
- **伝播**：`%w` で原因を包み、処理の文脈を加えられる
- **判定**：`errors.Is` / `errors.As` で、包まれた原因も調べられる

エラーを値として扱い、呼び出し元に判断を渡せる。

<span class="small">参考：[Working with Errors in Go](https://go.dev/blog/go1.13-errors) ／ [Effective Go: Errors](https://go.dev/doc/effective_go#errors)</span>

<!--
約40秒：失敗の経路が呼び出し元で見えること、エラーの値を包んで原因を検査できることを説明する。%wによる公開はAPI上の約束になり得る。エラーの確認漏れや記述量といった負担は最後にまとめる。
-->

---

# サーバーの仕事に道具が揃う

<div class="columns">
<div>

## リクエスト処理

- `net/http`：HTTPの送受信
- `context`：期限と中断を伝える
- goroutine：I/O待ちを並行処理

</div>
<div>

## 開発・運用

- `-race`：データ競合の検出
- `pprof`：CPU・メモリの調査
- `go build`：実行ファイルを生成

</div>
</div>

<span class="small">参考：[Go for Cloud & Network Services](https://go.dev/solutions/cloud) ／ [Race Detector](https://go.dev/doc/articles/race_detector) ／ [pprof](https://pkg.go.dev/net/http/pprof)</span>

<!--
約50秒：HTTPや並行処理だけでなく、タイムアウト・競合調査・プロファイリングまで、サービス開発で必要な道具が一通り揃う。race detectorは実行された経路の競合を見つける補助で、競合がないことの証明ではない。pprofの公開範囲は適切に制限する。
-->

---

# 例：リクエスト終了をDB処理に伝える

```go
ctx, cancel := context.WithTimeout(r.Context(), 2*time.Second)
defer cancel()

user, err := repo.FindUser(ctx, id)
if err != nil {
    return err
}
```

クライアントの切断やタイムアウトを、下流の処理にも伝える。

<span class="small">参考：[Canceling in-progress operations](https://go.dev/doc/database/cancel-operations)（コードはイメージ）</span>

<!--
約1分：HTTPリクエスト由来のcontextに2秒の期限を加え、DB層へ渡す例。repo.FindUserはcontextに対応したDB APIを使う想定。キャンセルは協調的で、渡すだけで任意の処理が強制停止するわけではない。リクエストが不要になった後の資源消費を減らせる。サンプルは関数内の抜粋で、handlerで直接return errする形ではない。
-->

---

# 並行処理は「開始」と「終了」をセットで考える

<div style="display:flex; align-items:center; gap:35px; margin:35px 0;">
<div><strong>APIリクエスト</strong><br>期限を設定</div>
<div style="color:#61a996;">→</div>
<div><strong>複数の処理</strong><br>goroutineで実行</div>
<div style="color:#61a996;">→</div>
<div><strong>結果を集約</strong><br>失敗・終了を処理</div>
</div>

- 先に失敗したら、残りをキャンセルできるようにする
- 無制限に起動せず、外部APIやDBへの同時実行数を制御する

<span class="small">参考：[Go Concurrency Patterns: Context](https://go.dev/blog/context) ／ [errgroup](https://pkg.go.dev/golang.org/x/sync/errgroup)</span>

<!--
約50秒：goroutineは起動しやすいが、終了条件や同時実行数の管理が必要。errgroup.WithContextやSetLimitなどはその助けになる。キャンセルに下流が対応していることも必要。Javaの仮想スレッドなど他言語にも同様の課題と手段がある。
-->

---

# アーキテクチャ：依存の向きを制御する

```go
type UserReader interface {
    FindUser(ctx context.Context, id string) (User, error)
}
```

- 業務ロジックは**必要な操作**に依存する
- DB実装とテスト用の実装が、同じ振る舞いを提供できる
- インターフェースは境界に置き、すべての型に作らない

<span class="small">参考：[Effective Go: Interfaces](https://go.dev/doc/effective_go#interfaces) ／ [Code Review Comments](https://go.dev/wiki/CodeReviewComments#interfaces)</span>

<!--
約55秒：たとえば業務側が必要とするFindUserだけをinterfaceで表し、DB実装を注入する。明示的なimplements宣言なしにメソッド集合が一致すれば満たせる。Consumer側の関心に合わせた小さな境界はテストに役立つが、Goであるだけでクリーンアーキテクチャになるわけではない。具体型のままで十分な箇所もある。
-->

---

# ビルドしてサービスを届ける

- OS・CPUを指定して、実行ファイルをビルドできる
- 実行ファイルをコンテナに配置し、環境へ届けやすい
- 実行先にGo SDKのインストールは不要

<span class="small">参考：[Compile and install](https://go.dev/doc/tutorial/compile-install) ／ [Go Security](https://go.dev/security/)</span>

<!--
約35秒：Goの実行ファイルにはGoランタイムが含まれる。cgoによる動的リンクやCA証明書、設定ファイルなどの依存が残る場合がある。監視や正常終了といった運用上の課題は後でまとめる。
-->

---

# マイクロサービスとの相性

- HTTPの送受信と並行処理を、標準の道具で扱える
- サービスごとにビルドし、独立して配布できる
- インターフェースで処理の境界を表現できる

<span class="small">参考：[Go for Cloud & Network Services](https://go.dev/solutions/cloud)</span>

<!--
約35秒：HTTPやコンテナへの配布といった特性が、独立したAPIサービスを作るときに役立つ。モノリスにもGoを使える。分割に伴うテスト・監視・通信の複雑さは最後にまとめる。
-->

---

# Goコミュニティ：共通の道具と知見を育てる

- 言語・標準ライブラリの大きな変更は **proposal** で議論
- カンファレンスやユーザーグループで事例・知見を共有
- 2025年の調査では **5,379人** が回答<br>その **55%** がCLIとAPIサービスの両方を開発

<span class="small">数値は調査回答者に限った結果です。<br>出典：[Contribution Guide](https://go.dev/doc/contribute) ／ [Go Conference](https://gocon.jp/) ／ [2025 Go Developer Survey](https://go.dev/blog/survey2025)</span>

<!--
約40秒：Goの文化はgofmtのような道具だけではなく、議論やレビュー、学びの場にも現れる。調査は公募とIDE経由で集まった回答者の結果であり、Go開発者全体の代表値とは限らない。Go Conferenceは日本のコミュニティの一例。
-->

---

# AI時代にGoをどう見る？

**仮説**：書式や標準的な道具が揃うと、生成後の確認を共通化しやすい。

- 定型コードやテストのたたき台は、AIに任せやすい
- `gofmt`・型検査・テストで、機械的に確認できる部分がある
- 設計、競合、セキュリティ、テストの妥当性は人が判断する

<span class="small">Goが他言語よりAIに適するという比較結果を示すものではありません。<br>参考：[2025 Go Developer Survey](https://go.dev/blog/survey2025)</span>

<!--
約50秒：これはGoの道具立てからの仮説であり、他言語との生成精度比較ではない。2025年のGo開発者調査では、回答者からテストや定型コード生成などの利点が挙がる一方、動かないコードや品質への懸念も報告された。AIが書いたテストも誤った仕様を固定することがあるため、差分と期待する振る舞いをレビューする。
-->

---

# 最後に：デメリットと注意点

- `if err != nil` が繰り返され、エラー処理の記述が増えやすい
- シンプルな文法でも、設計・命名・並行処理の難しさは残る
- 実行ファイルにも、証明書や外部ライブラリなどの依存が残る場合がある
- マイクロサービス化すると、通信・監視・テストの負荷が増える

<span class="small">参考：[Go Errors](https://go.dev/blog/go1.13-errors) ／ [2023 Go Developer Survey](https://go.dev/blog/survey2023-h2-results)</span>

<!--
約50秒：明示的なエラー処理は繰り返しも増える。race detectorはすべての競合を証明するものではなく、goroutineの終了や共有状態の設計も必要。配布先にはcgoの動的ライブラリ、CA証明書や設定が必要な場合がある。マイクロサービスの運用負担は言語ではなく構成による。2023年調査では、回答者がテスト・デバッグや運用の複雑さを課題に挙げた。
-->

---

# Goを選ぶ判断軸

- API・CLIなどを**小さなチームで継続的に運用**する
- 書式や標準的な道具を揃え、コードを共有しやすくしたい
- エラー処理の記述量や既存資産、チームの経験も含めて選ぶ

まずは既存の小さなAPIを題材に、実装から観測まで試す。

<span class="small">参考：[Go公式FAQ](https://go.dev/doc/faq) ／ [Go Developer Survey 2025](https://go.dev/blog/survey2025)</span>

<!--
約40秒：決め手は性能の数字だけではなく、チームで開発・運用し続けられるか。比較する際は同じ要件で、可読性、既存ライブラリ、デプロイ、観測、障害時の振る舞いを確認する。
-->
