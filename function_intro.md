---
title: "関数入門"
layout: page
---

kotlinの関数について、とりあえずActivityの中でコードをメンバ関数に抜き出していくのに必要な程度の入門をする。

## 関数という概念自体はJS入門でやってください

関数というのを他の言語でやった事が無い人、具体的には仮引数やreturnというのが何なのかわからない人は、
以下のJS入門の 第七回〜第九回 をやってください。 

- [第七回: 関数その1 関数を作る事、呼び出す事 - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch07.html)
- [第八回: 関数その2 関数からもらうもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch08.html)
- [第九回: 関数その3 関数に渡すもの - 算数で挫折した人向けの、Javascript入門](https://karino2.github.io/js-introduction/ch09.html)

## kotlinの関数の定義の仕方

関数は以下のように定義します。

{% capture func_intro1 %}

fun sum(a: Int, b:Int) : Int {
  return a+b
}

fun main() {
  println(sum(1, 2)+3)
}
{% endcapture %}
{% include kotlin_quote.html body=func_intro1 %}

さて、関数を定義しているのは以下の部分になります。

```kotlin
fun sum(a: Int, b:Int) : Int {
  return a+b
}
```

### kotlinの関数は、仮引数とreturnの型を指定する必要がある

JS入門との一番の違いとしては、仮引数とreturnの型を、最初に指定する必要があります。

仮引数の方は変数名（上の例だとaとb）の後にコロンを置いて、その後に型を置きます。

中括弧の前がreturnの型です。
return文を見なくても、funの行だけで引数とreturnの型が分かるようになっているのが特徴です。

例えばreturnの所でtoStringして戻りの型をStringに変えてみましょう。
そうするとこうなります。

{% capture func_intro2 %}

fun sum2(a: Int, b:Int) : String {
  return (a+b).toString()
}

fun main() {
  // 6じゃなくて"33"になる事に注目
  println(sum2(1, 2)+3)
}
{% endcapture %}
{% include kotlin_quote.html body=func_intro2 %}

### returnしない場合は戻りの型は無しで良い

printlnだけする関数など、returnするものが無い関数では、戻りの型は書かなくても良いです。

{% capture func_noreturn %}

fun printHello() {
  println("Hello World")
}

fun main() {
  printHello()
}
{% endcapture %}
{% include kotlin_quote.html body=func_noreturn %}

なお、戻りの型を書かないと、何も無いを意味するUnit型というものになります。

つまり、printHelloは以下のように書いても全く同じ意味になります。

```kotlin
fun printHello() : Unit{
  println("Hello World")
}
```

### 関数の名前は小文字始まりで単語境界は大文字にする決まり

これは破る事もあるのだけれど、原則関数の名前は小文字で始めて、単語の区切りは大文字にする。
つまりprintHelloとかsetupDataとか。

これは歩道は左側通行、程度には破られるルール。人があんま居ない時はまぁどこ歩いてもいいでしょう。

## これまでのコードを関数を使ってみよう

関数を学ぶには使ってみるのが一番。けれどなんか人工的な例を出すのは盛り上がりに欠けるので、これまで書いたコードを関数を使って直してみよう。

なお、以下では関数にしてくくりだせ、と言ったら、基本的には「メンバ関数」というものにします。
置き場所としては[コードの置き場所入門](code_location_intro.md)で言う所の「2番目の区画」になります。

### ListViewの表示編のfor文を関数にする

[ListView表示編の二回目以降](listview_disp_second.md)では、以下のようなコードがあったと思います。

```kotlin
    val listData = mutableListOf()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        for(i in 1..100) {
          listData.add("${i}番目の項目です")
        }

        findViewById<ListView>(R.id.listView1).adapter = adapter
    }
```

このうち、以下の部分をsetupDataという関数にして、それを呼び出すように変えてください。

```kotlin
  for(i in 1..100) {
    listData.add("${i}番目の項目です")
  }
```

### 簡易電卓のonClickListenerの処理を関数にする

[簡単な電卓を作ってみよう](simple_calc.md)で、9個のボタンの処理がほとんど同じになっていたと思う。
これを関数にして共通化してみよう。