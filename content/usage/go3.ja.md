---
title: "条件演算子と条件文"
# description: "https"
date: 2020-01-27T00:36:14+09:00
draft: false
---

## 条件演算子
条件演算子には，`>`（大なり）,`<`（小なり）,`==`（等しい）,`!=`（等しくない）,`<=`（小さいか等しい）,`>=`（大きいか等しい）がある。また，`&&`（かつ）,`||`（または）もあるが，これらはboolタイプの結果を二つ要する二項演算子である。`&&`は両者が真である場合にTrueに，`||`の場合は，どちらかの一方が真である場合，Trueを返す。以下のコードを実行すると`true`を返す。

{{< highlight Go >}}
package main

import (
    "fmt"    
)
func main() {
	a := 1 < 2 && 3 < 4

    fmt.Println(a)
}
{{< /highlight >}}

## 条件文
「もし，雨が降るのであれば，傘の売れ行きが伸びるだろう。」というのが条件文である。

まず`if`を利用して条件を書く（もし，雨が降るのであれば）。その後，条件を満たした場合（true）はどのようにするかを書く。その次に，条件を満たさない場合（false）はどのようにするのかを書く。

{{< highlight Go >}}
package main

import (
    "fmt"    
)
func main() {
	a := 1
    // aが3と等しければ…
    if a == 3 {
    // 次の文を出力する。
        fmt.Println("aは3です。")
    }
    // 等しくなければ，次の文を出力する。
    fmt.Println("aは3ではないです。")
}
{{< /highlight >}}

上記は，`else`を利用して次のように書くこともできる。

{{< highlight Go >}}
package main

import (
    "fmt"    
)
func main() {
	a := 1
    // aが3と等しければ…
    if a == 3 {
    // 次の文を出力する。
        fmt.Println("aは3です。")
    // 等しくなければ，次の文を出力する。
    } else {
        fmt.Println("aは3ではないです。")
    }
}
{{< /highlight >}}

ときには，条件を増やしたい場合もある。「ウーロン茶ください。もし，なければ緑茶をください。それもなければ，水でいいでｓ。」のように。これを表すためには，`if`と`else`を組み合わせる。

{{< highlight Go >}}
package main

import (
    "fmt"    
)
func main() {
	a := 1
    // aが3と等しければ…
    if a == 3 {
    // 次の文を出力する。
        fmt.Println("aは3です。")
    // aが2と等しければ，次の文を出力する。
    } else if a == 2 {
        fmt.Println("aは2です。")
    } else {
    // aが3でも2でもない場合，以下を出力する。
        fmt.Println("aは3でも，2でもないです。")
    }
    // 以下は条件文と関係ないので，なくても問題ない。
    fmt.Println("aは", a, "です。")
}
{{< /highlight >}}

`else if`は一つ以上あってもいいし，一番最後の`else`はあってもいいし，なくてもいい。

## 四則演算する計算機

{{< highlight Go >}}
package main

import (
    "fmt"
    // 標準入力のため
    "os"
    // 入力から1行をもらうため
    "bufio"
    // 文字列から入らない部分を削除するため
    "strings"
    // stringをintに変えるため
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

    if line == "+" {
        fmt.Printf("%d + %d = %d\n", n1, n2, n1 + n2)
    } else if line == "-" {
        fmt.Printf("%d - %d = %d\n", n1, n2, n1 - n2)
    } else if line == "*" {
        fmt.Printf("%d * %d = %d\n", n1, n2, n1 * n2)
    } else if line == "/" {
        fmt.Printf("%d / %d = %d\n", n1, n2, n1 / n2)
    } else {
        fmt.Println("入力が間違っています。")
    }
}
{{< /highlight >}}

