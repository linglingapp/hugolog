---
title: "配列"
# description: "https"
date: 2020-01-19T00:36:14+09:00
draft: false
---

## 配列の宣言
変数には一つ以上の値を入れることもできる。配列（array）を使えば，たくさんの値を格納することができる。
고에 대해 알아보도록 하자.
{{< highlight Go >}}
package main

import "fmt"

func main() {
    // 箱（配列）を10個作ろう。箱のタイプは整数タイプ。
    var A [10]int

    // len(A)は，変数Aの長さ（length）を意味する。
    // つまり，len(A)は10と書くのと同じ。
    for i := 0; i < len(A); i++ {
        // Aという箱にi*iの値を入れる。
        A[i] = i * i
    }
    fmt.Println(A)
}
{{< /highlight >}}

ちなみに，文字列（string）も配列である。「Hello world!」は12byteからなっている配列である。英数字1文字やスペース一つは，1byteに該当する。Go言語では，UTF-8（1～4byteを使用してUnicode文字を表す）を使用する。ASCII文字は，1byteで済むが，漢字やハングルなどは，2～4byteが必要である。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    s := "Hello World!"

    for i := 0; i < len(s); i++ {
        fmt.Print(s[i], ", ")
    }
}
{{< /highlight >}}

上記を実行すると「Hello world!」の文字コード，つまり，`72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100, 33`が現れる。英語は1byteからなっているのがわかる。一方，漢字や平仮名，ハングルは3byteである。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    s := "Hello 世界!"

    for i := 0; i < len(s); i++ {
    fmt.Print(s[i], ", ")
    }
}
{{< /highlight >}}

`72, 101, 108, 108, 111, 32, 228, 184, 150, 231, 149, 140, 33`と出力される。「世界」に該当する部分は`228, 184, 150, 231, 149, 140`で，1文字3byteである。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    s := "Hello 世界!"
    // runeは変数のタイプの一つ。
    // runeタイプは1～3byteの長さを持つ。
    // runeタイプは文字（英語・漢字など）によって長さが変わる。
    s2 := []rune(s)
    for i := 0; i < len(s2); i++ {
    fmt.Print(s2[i], ", ")
    }
}
{{< /highlight >}}

これの結果は`72, 101, 108, 108, 111, 32, 19990, 30028, 33`である。これは合計13byteであり，9byteではない。`19990`や`30028`，つまり，「世」と「界」はそれぞれ3byteである。
`rune`はいつ使うのだろうか。文字列の長さを文字単位で求めたい場合，`rune`タイプにしないと，1byte以上の文字は文字化けする。言い換えると，「世」に該当する3byteが`228, 184, 150`と出力されるので，文字化けするのである。これらを一つにまとめるために`rune`を使うと，「世」は`19990`となり，文字化けしない。

{{< highlight Go >}}
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {
    world1 := "world"
    world2 := "世界の平和"
    fmt.Println(utf8.RuneCountInString(world1))
    fmt.Println(utf8.RuneCountInString(world2))
}
{{< /highlight >}}

上記を実行すると，いずれも`5`となる。それぞれの文字列が五つの`rune`を持つのである。

## 配列のコピー
配列をコピーしたい場合は，どのようにすればいいのだろうか。早速例を見てみよう。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    // 新しい配列を作り，数字を格納
    arr := [5]int{1, 2, 3, 4, 5}
    // また新しい配列を作る
    clone := [5]int{}

    for i := 0; i < 5; i++ {
        // 配列をコピー
        clone[i] = arr[i]
    }
    fmt.Println(clone)
}
{{< /highlight >}}

上記は配列を順序もそのままコピーした。今度は逆順にコピーしてみよう。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    // forはarrの長さの半分繰り返す
    for i := 0; i < len(arr)/2; i++ {
        // 二重代入を使う
        // 左のarr[i]にはarr[len(arr)-1-i]が入る
        // 左のarr[len(arr)-1-i]にはarr[i]が入る
        arr[i], arr[len(arr)-1-i] = arr[len(arr)-1-i], arr[i]
    }
    fmt.Println(arr)
}
{{< /highlight >}}

真ん中の3はそのままなので，いじらなくてもいい。

## Radix Sort
※　以下の内容は，やや専門的なところに踏み込むので，飛ばしても関係ない。

RADIXアルゴリズムは，単純，かつ，速い。元素の範囲が十分に限定されており，配列の個数が少ないときに適切である。「元素の範囲」は，たとえば，「1から100まで」，または，「-100000から100000まで」のようなものを意味する。後者のように，元素の範囲が広い場合，RADIXアルゴリズムは不利である。ソートをするアルゴリズムはRADIX以外にもいくつかあるので，このように元素の範囲が広いときには，他のアルゴリズムを使うのがいいだろう。

以下のプログラムを実行すると，配列に入っている`{0, 5, 4, 9, 1, 2, 8, 3, 6, 4, 5}`が`[0 1 2 3 4 4 5 5 6 8 9]`のように，ソートされる。

{{< highlight Go >}}
package main

import "fmt"

func main() {
    // 以下の配列をソートしたい
    arr := [11]int{0, 5, 4, 9, 1, 2, 8, 3, 6, 4, 5}
    // 上記の配列をソートするために必要な箱は，0から9までの10個
    // arr配列に10は入っていないので，temp配列に無駄な箱（10）を作る必要はない
    temp := [10]int{}

    for i := 0; i < len(arr); i++ {
        // arr配列に入っている数字を，順番に読み込み，変数（index）に格納する
        index := arr[i]
        // arr配列にある数字が何個あったのかを数えて，temp配列に格納する
        temp[index]++
    }

    idx := 0
    // temp配列の長さより小さければ，繰り返す
    for i := 0; i < len(temp); i++ {
        // 現在，temp配列は[1 1 1 1 2 2 1 0 1 1]となっている
        // なので，最大の繰り返しの回数は2回
        for j := 0; j < temp[i]; j++ {
            // 最初はarr配列の最初の箱に入っている数字（0）を（0）に書き換える
            // arr配列の2番目に入っている数字は5だが，i（1）に書き換える
            // 同じ数字が二つある場合，temp配列はそれを2で表している
            // jが2より小さい場合，作業は2回繰り返されるので，
            // arr配列には同じ数字が連続して2回プッシュされる
            arr[idx] = i
            // 5，6番目の配列（arr[idx]）に4（i）は2回プッシュされる
            idx++
        }
    }
    fmt.Println(arr)
}
{{< /highlight >}}

数字以外には，たとえば，ローマ字で書かれている人の名前をアルファベットの順番で並び替えることができる。RADIXソートは使用場面が限られているので，プログラミングをする前に，まず，この方法が使用可能なのかどうかを確認しておいた方がいいだろう。