---
title: "変数"
# description: "https"
date: 2020-01-28T00:36:14+09:00
section: post
draft: false
---

## 変数のタイプ
*y* = *x* + 1において，*x*と*y*は値が変わり得るので「変数」と言う。プログラミングにおける「変数」もそうである。代入する値によって，箱である変数も変化する。

変数の概念を理解するためには，簡単な算数をやってみるとよい。`//`で始まっているところは，注釈なので，プログラムを実行すると無視される。なので，わざわざそれまで書く必要はない。


{{< highlight Go >}}
// プログラムの基準点になるファイルには，以下のように書いておく。
package main

// 画面に文字列を出力

// main関数（function）を作ってみよう。
func main() {
    // varは「変数の宣言」を意味する。var aと書くと，名前がaの変数を宣言することになる。
    // int は integer，つまり，「整数」の略である。
    // var a int が意味するのは，「aという変数を宣言する。ただし，aのタイプは整数」である。
    var a int
    var b int
    // 空っぽの変数aとbに，整数を入れてみよう。
    a = 1
    b = 2
    // printは「画面に出力せよ」ということである。printlnは「画面に出力して一行開けろ」という意味。
    println(a + b)
}
{{< /highlight >}}

以上のようにコードを書いて実行してみよう。ここではファイル名を`math`にしている。

    go run math.go

問題なければ，画面には3が表示される。

### int（整数）
プログラミングでの変数は，タイプ（type）という属性を持つ。先ほどの変数aとbは，`int`だからタイプは「整数」である。ここに他のタイプ，たとえば，文字列「こんにちは」を入れることはできない。

### string（文字列）
変数に文字列を入れたいのであれば，変数のタイプを変更する必要がある。変数に文字列を入れるためには，`int`の代わりに`string`を使えばよい。ここで注意すべき点は，文字列を変数に入れるときに，文字列の前後にダブルクォーテーション（""）を付けることである。

{{< highlight Go >}}
package main

func main() {
    var a string
    var b string
    a = "こんにちは"
    b = "！"
    println(a + b)
}
{{< /highlight >}}

これを実行してみると，「こんにちは！」が表示される。

### float（浮動小数）
整数と浮動小数は，どちらも数字だから同じタイプとして考えるかもしれないけど，違うタイプである。Go言語には`float32`と`float64`がある。それぞれ，4バイトと8バイトを表す。

### bool（ブーリアン型）
`bool`はブーリアン型（Boolean）で，`true`と`false`の二つがある。初期値は`false`。

## 面倒くさい
変数のタイプを毎回宣言するのは，面倒くさい。勝手に値のタイプを判断して，変数に入れてほしい。

    var a int
    a = 1

この2行は，次の1行で済む。

    var a = 1

このように書けるのは，整数に限った話ではない。

    var a = 3.14

`var`を書くのも面倒なので，もっと短くしてみよう。

    a := 1

これは，先ほどの`var a = 1`と同じである。このようにすると，`var`と`int`を使ってわざわざタイプを宣言しなくて済む。楽々。

色んな変数がある場合，紛らわしいかもしれない。そのようなときには，以下のようにして，どのようなタイプなのかを書いておくのが役に立つかもしれない。

    var a int = 1

## Go言語のPrint
Printについて少しだけ補足。`fmt`を利用するので，次のようにインポートしておく。

{{< highlight Go >}}
package main

import (
    "fmt"
)
{{< /highlight >}}

そして，aとbという変数に適当な値を割り当てて，次のようにしてみよう。変数に値を入れる方法がわからなければ，上にあるのでそちらに戻ろう。

    fmt.Printf("%v\n", a + b)

このようにすると，""で囲ったところには，aとbを足した値が入り，画面に表示される。`a + b`がそのまま文字列として入るのではない。`%v`は，`a + b`の結果が「整数」「浮動小数」「文字列」等の中でどのタイプなのか，自ら判断する。`\n`は改行。

上にある1行は，次のようにすることと同じである。

    fmt.Println(a + b)

ところで，`fmt`はピリオドで`Printf`や`Println`とつながっている。これは，`fmt`というパッケージから`Printf`や`Println`という機能を読み込んで使用する，という意味である。
`fmt`パッケージには，他にどのような機能があるのか，[ここ](https://golang.org/pkg/fmt/)で確認できる。

ついでに，`Println`という機能を試してみよう。まずは，どのように使えるのかを確認。

    func Println(a ...interface{}) (n int, err error)

何かわからないやつが出てきた。`a ... interface{}`と，その右側にある`err error`が何かわからない。文法の下には説明も書いてあるので，それも見てみる。

> Println formats using the default formats for its operands and writes to standard output. Spaces are always added between operands and a newline is appended. It returns the number of bytes written and any write error encountered.

わかるようなわからないような…

さらにその下には例が書いてある。外国語学習のときもそうだが，単語の説明を見てわからなかったのが，用例を見て「あ，わかった！」となる瞬間があると思う。プログラミング言語もいっしょだ。

[ここ](https://golang.org/pkg/fmt/#Println)で`Example`を押すと，次の例が現れる。

{{< highlight Go >}}
package main

import (
	"fmt"
)

func main() {
	const name, age = "Kim", 22
	fmt.Println(name, "is", age, "years old.")

	// It is conventional not to worry about any
	// error returned by Println.

}
{{< /highlight >}}

例といっしょに右下には`Run`というボタンがある。押してみると「Program exited.」というメッセージとともに，`Kim is 22 years old.`が表示される。総合的に考えて見ると，`const`というのが何かはわからないが，なんとなく，`name`と`age`にそれぞれ`Kim`という文字列（string）と`22`という整数（int）が入るような気がする。そして，それらの変数が`fmt.Println`というパッケージの機能で「name」と「age」という文字列といっしょに出力されるのではないだろうか。試しに，`Kim`と`22`を`太郎`と`100`に変えてプログラムを実行してみる。すると，次のように表示される。

    太郎 is 100 years old.

Voilà!