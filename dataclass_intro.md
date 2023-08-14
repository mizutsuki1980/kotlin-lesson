---
title: "data class入門"
layout: page
---
今回はdata classの話をします。
クラスの話はもうちょっと後にするつもりなのだけど、とりあえずdata classは使いたいのでdata classの簡単な使い方を先に。

[Data classes - Kotlin Documentation](https://kotlinlang.org/docs/data-classes.html)

## ListViewに挑む！編集編に、投稿時間もつけたい

data classを使いたくなるシチュエーションとして、[ListViewの編集編](listview_edit.md)で作ったものに対し、
アイテム追加時の時間も表示したい、と考えたとします。

けれど、リストのアイテムは現状、String型となっている。

```kotlin
val listData = mutableListOf<String>()
```

ここに、StringとDateの二つを追加していきたい。そういう時に使うのがdata classです。

data classは、リストに二つ以上の項目を同時に追加していきたい時に使います。

## data classの簡単な例

まずdata classの例を見ていきたいと思います。

data class は以下のように使います。

{% capture dataclass_intro1 %}
data class TwoInt(val v1: Int, val v2: Int)

fun main() {
  val ti1 = TwoInt(1, 2)

  println(ti1.v1)
  println(ti1.v2)
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro1 %}

以下、細かく見ていきましょう。

### data classの文法

まずdata classは最初に`data class クラス名`で始まります。
クラス名は大文字始まりで単語の区切り大文字という慣習になっています。
つまりTwoIntとかUserDataとかそういうものになります。

そして`クラス名`の後にカッコが続き、変数定義のようなものが、型付きで並びます。

代入する場合はvar、代入しない場合はvalですが、それについては後で説明します。

こうしてdata classが定義出来ます。data classは、新しい型となります。この場合はTwoInt型が出来る事になります。

次に使い方です。data classは定義すると、そのクラス名を関数のように使う事が出来て、
そのreturnはそのdata classのデータとなります。先ほどの例では`TwoInt`というのを関数のように使う事が出来て、
`TwoInt(1, 2)`とすると、TwoInt型のデータが作れる訳です。

### 各構成要素の触り方はドットと変数名

さて、`val ti1 = TwoInt(1, 2)` とti1変数にデータを入れられました。
この1とか2にはどうアクセスするか？というと、`ti1.v1`とか`ti1.v2`というように、
ドットとdata classを定義した時の変数名でアクセスします。

これで基本的な使い方は終わりです。
ちょっと幾つか例とか練習問題とかを見ていきましょう。

### data classを使った例いろいろ

幾つか例を見ていきましょう。さきほどはどっちもIntだったので、次はStringとIntの例を。
ユーザー名とスコアを持たせる、というのは割と良くあります。

{% capture dataclass_intro2 %}
data class User(val name: String, val score: Int)

fun main() {
  val user1 = User("ほげいか", 95)
  val user2 = User("karino2", 72)

  println("${user1.name}の点数は${user1.score}です")
  println("${user2.name}の点数は${user2.score}です")
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro2 %}

また、これをListに入れる事もできる。

{% capture dataclass_intro3 %}
data class User(val name: String, val score: Int)

fun main() {
  val mlist = mutableListOf<User>()

  mlist.add(User("ほげいか", 95))
  mlist.add(User("karino2", 72))

  for(user in mlist) {
    println("${user.name}の点数は${user.score}です")
  }
}
{% endcapture %}
{% include kotlin_quote.html body=dataclass_intro3 %}


## クラスの用語いろいろ

このシリーズはAndroid側とkotlin言語側の話が交互に続いていく感じになっていますが、
Android側の最難関はおそらくListViewです。

一方kotlin側の最難関はクラスとなります。

クラスは何が難しいって用語がめちゃくちゃ多い。
そこで割と単純なdata classの段階で用語を幾つか先取りしておいて、クラスを勉強する時に少し楽になるようにここで少し頑張っておきましょう。

### クラスとオブジェクト（またはインスタンス）

`data class TwoInt(val v1: Int, val v2: Int)`でTwoIntクラスという名前の型が出来ます。
そして、TwoIntクラスの型のデータをオブジェクトと呼びます。

少しややこしいですね。[型とは何か？](what_is_type.md)で少し触れたように、
型というのは、データのカテゴリでした。

ストII、ストIIダッシュ、スーパーストII、スパIIXなどがデータで、ストIIシリーズが型、というものでした。

つまり一つの型というカテゴリに対し、そのカテゴリにあてはまるデータが複数ある訳です。

そしてこのカテゴリには`Int`とか`String`とか`List<String>`とかがある訳だけれど、
今回新しく`TwoInt`というカテゴリを作れるようになった訳です。

ストIIシリーズという型にはいろんなストIIのタイトルがあるように、
TwoIntというカテゴリにも、`(1, 2)`というデータを持つTwoIntや、`(8, 3)`を持つTwoIntなど、いろんなデータが存在します。

このデータを、クラスの時は「オブジェクト」とか「インスタンス」と呼ぶ事になっているという訳です。
「オブジェクト」と「インスタンス」はほぼ同じ意味です。起き昇とリバーサル昇竜くらいの違いです。

最初のうちはややこしいかもしれないけれど、クラスを自分で作っていくようになるとこのインスタンスとクラスの区別みたいなのは何度も出てくるようになるので、
使っていくと慣れます。

という事で我らもこれから使っていく事にする。

以上をまとめると、以下のようになります。

- `data class TwoInt(val v1: Int, val v2: Int)`でTwoIntクラスが出来る
- `TwoInt(1, 2)` などと呼ぶと、TwoIntクラスのインスタンスが作れる
- `TwoInt(7, 3)` などと呼んでも、TwoIntのクラスのインスタンスは作れる
- 一つのTwoInt型からインスタンスはたくさん作れる
- TwoInt型とTwoIntクラスはほとんど同じ意味

とりあえず用語はややこしいので何度も使って慣れる方がいい。という事で以後バリバリ使っていきましょう。

### メンバ変数v1, v2

先ほどの定義を再掲します。

```kotlin
data class TwoInt(val v1: Int, val v2: Int)
```

このv1とv2をメンバ変数と呼びます。
メンバ変数というのはこれまでもActivityの所に定義している変数をそう呼んでいました。
あれも内部的には全く同じものです。
クラスを真面目にやると両者が同じものなのが分かりますが、とりあえずここではdata classのこれらの変数もメンバ変数と呼ぶ、
とだけ覚えておいてください。