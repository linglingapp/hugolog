---
title: "switch-caseとfor文"
# description: "https"
date: 2020-01-24T00:36:14+09:00
draft: false
---

## switch-case
`if`と`else`を何回も繰り返して書くのは面倒だ。もっと短く書きたい。そこで使うのは`switch`と`case`である。

{{< highlight Go >}}
package main

import (
    "fmt"
    "os"
    "bufio"
    "strings"
    "strconv"
)

func main() {
    fmt.Println("演算する数字を入力してください。")
    reader := bufio.NewReader(os.Stdin)
    line, _  := reader.ReadString('\n')
    line = strings.TrimSpace(line)
    n1, _ := strconv.Atoi(line)

    fmt.Println("数字をもう一つ入力してください。")
    line, _ = reader.ReadString('\n')
    line = strings.TrimSpace(line)
    n2, _ := strconv.Atoi(line)

    fmt.Printf("入力した数字は，%d，%dです。", n1, n2)

    fmt.Println("続いて，演算子を「+, -, *, /」の中から選んでください。")

    line, _ = reader.ReadString('\n')
    line = strings.TrimSpace(line)

    switch line {
    case "+":
        fmt.Printf("%d + %d = %d\n", n1, n2, n1 + n2)
    case "-":
        fmt.Printf("%d - %d = %d\n", n1, n2, n1 - n2)
    case "*":
        fmt.Printf("%d * %d = %d\n", n1, n2, n1 * n2)
    case "/":
         fmt.Printf("%d / %d = %d\n", n1, n2, n1 / n2)
    default:
        fmt.Println("入力が間違っています。")
    }
}
{{< /highlight >}}

`switch`にあるものと一致するのかを，一番上にある`case`から確認していく。もし，一致するのであれば，以下の`case`はチェックせずに，関数を抜け出す。したがって，条件に一致するケースが二つ以上並んでいる場合，最も上にある`case`が優先的に実行されるのである。
なお，`switch`の値は省略できる。その場合，値は`true`と設定される。

## for文
他の言語には，`for`と`while`の二つがある場合が多い。しかし，Go言語には`for`しかない。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    i := 0
    for i < 10 {
        fmt.Println(i)
        // 以下は「i = i + 1」や「i += 1」と同じである。
        i++
    }
    fmt.Println("最終的なiの値は", i, "です。")
}
{{< /highlight >}}

for文には，「前処理文」「条件文」「後処理文」を表現することができる。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    // 前処理文; 条件文; 後処理文
    for i := 0; i < 10; i++ {
        fmt.Println(i)
    }
    fmt.Println("最終的なiの値は", i, "です。")
}
{{< /highlight >}}

しかし，上記のコードを実行するとエラーが生じる。iのスコープが問題となっているからだ。
    for i := 0
以上のようにfor文の中で設定したiは，for文の外で使うことはできない。iのスコープはfor文の中までである。このとき，変数の宣言はfor文の外ですればいい。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    // for文の外でiを宣言。
    var i int
    // 前処理文（iに値を代入。宣言ではない。「:=」ではないことに注意。）; 条件文; 後処理文
    for i = 0; i < 10; i++ {
        fmt.Println(i)
    }
    fmt.Println("最終的なiの値は", i, "です。")
}
{{< /highlight >}}

変数の名前が同じくiになっているので紛らわしい。では，スコープの違いを浮き彫りにするために，変数の名前を変えてみよう。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    // for文の外でjを宣言。
    var j int
    // 前処理文; 条件文; 後処理文
    for i := 0; i < 10; i++ {
        fmt.Println(i)
    }
    fmt.Println("最終的なjの値は", j, "です。")
}
{{< /highlight >}}

「最終的なjの値は 0 です。」と出力される。つまり，iとjは異なる変数，ということである。可能であれば，変数名は人間が読んで紛らわしくない名前にするのがいい。

## breakとcontinue
for文に条件を書かないと，`true`として認識し，無限ループとなる。また，for文は，`break`と`continue`と組み合わせる場合も多い。

`break`を使うことにより，iは10にならず，5になったときに，for文をbreakする。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    for i := 0; i < 10; i++ {
        if i == 5 {
            break
        }
        fmt.Println(i)
    }
}
{{< /highlight >}}

一方，`continue`は該当する値を飛ばして，次の作業を続ける。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    for i := 0; i < 10; i++ {
        if i == 5 {
            continue
        }
        fmt.Println(i)
    }
}
{{< /highlight >}}

## for文で九九

{{< highlight GO >}}
package main

import "fmt"

func main() {
    for base := 1; base <= 9; base++ {
        fmt.Printf("%dの段\n", base)

        for j := 1; j <= 9; j++ {
            fmt.Printf("%d * %d = %d\n", base, j, base * j)
        }
        // 「Println()」のように何も書かないと，一行空ける。
        fmt.Println()
    }
}
{{< /highlight >}}