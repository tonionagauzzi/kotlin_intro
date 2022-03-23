# Exercise (1) <span style="color: red;">解答</span>

## Hello World!

まずはこれ！ソースをコンパイルしてとりあえず動かすところまでやってみよう！

* 動作確認の方法については、[基本的な文法 - Kotlin Playground](./basic_syntax.md#kotlin-playground) を参照のこと。
* つまり、以下を Run してみよう！

```kotlin
fun main(args: Array<String>) {
    println("Hello world!")
}
```

## Fizz Buzz

基本的な文法、関数の使い方を抑えたところで、以下の練習をやってみよう！

* 入力は 1〜100
* 3の倍数の時は「Fizz」を出力
* 5の倍数の時は「Buzz」を出力
* 3の倍数かつ5の倍数の時は「FizzBuzz」を出力
* それ以外のときはそのまま数字を出力

* 出力例

```
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
...
FizzBuzz
91
92
Fizz
94
Buzz
Fizz
97
98
Fizz
Buzz
```

* ポイント
  * `if` 文、`for` ループが使えれば解けるはず
  * `when` を使って短く書いたりもできる

```kotlin
private fun fizzBuzz(number: Int): String {
    return when {
        number % 15 == 0 -> "FizzBuzz"
        number % 5 == 0 -> "Buzz"
        number % 3 == 0 -> "Fizz"
        else -> "$number"
    }
}

// 1行でも書ける
private fun fizzBuzz2(number: Int) = "${if (number % 3 == 0) "Fizz" else ""}${if (number % 5 == 0) "Buzz" else ""}".let { output -> if (output.isBlank()) "$number" else output }

fun main(args: Array<String>) {
    for (i in 1..100) {
        println(fizzBuzz(i))
    }
}
```

* 余裕があれば fizzBuzz 関数を書いて main を短くしよう！

## うるう年

入力された数字を西暦としたときに、うるう年かどうか判定する関数を書いてみよう！

* 入力は 0 以上の `Int`
* うるう年ならば `true`、そうでないならば `false` を返す
* ところでうるう年って？
  * 年が 4 で割り切れる場合、その年はうるう年として扱う。  
  ただし、100 で割り切れてかつ 400 で割り切れない年は除く
  * 1700、1800、1900、2100、2200、2300、2500、2600 はうるう年ではない
  * 1600、2000、2400 はうるう年

```kotlin
import java.security.InvalidParameterException

// うるう年かどうかを判定する関数
private fun isLeapYear(year: Int): Boolean {
    // 0未満はエラー
    if (year < 0) {
        throw InvalidParameterException("The parameter \"year\" should not minus.")
    }

    // うるう年は4で割り切れることが前提
    if (year % 4 != 0) {
        return false
    }

    // 100で割り切れて、400で割り切れないとうるう年ではない
    if (year % 100 == 0 && year % 400 != 0) {
        return false
    }

    return true
}

// もっと短く書ける
private fun isLeapYear2(year: Int) = if (year < 0) {
    throw InvalidParameterException("The parameter \"year\" should not minus.")
} else {
    year % 400 == 0 || (year % 100 != 0 && year % 4 == 0)
}

fun main(args: Array<String>) {
    for (i in 2000..2100) {
        if (isLeapYear(i)) {
            println("$i 年はうるう年です！")
        }
    }
}
```

* 結果を検証しよう！
  * https://citizen.jp/support-jp/manual/terms/deeper_01.html

## 累乗

数字をふたつ引数にとって (`a`、`n` とする)、a の n 乗を返す関数を書いてみよう！

* 入力は `a`、`n` 共に任意の数 (正の数でも負の数でも浮動小数点でも OK) とする
* …とすると考慮することが非常に増えるので、ここではひとまず、いずれも正の整数とする
  * 余裕があったら浮動小数点も取れるようにしてみよう！

```kotlin
import kotlin.math.pow

// nは整数しか対応していない版
fun power(a: Double, n: Double): Double {
    if (a < 0 || n < 0) {
    	println("inputs must be positive. return 0")
    	return 0.0
    }
    
    var res = 1.0
    for (i in 1..n.toInt()) {
        res *= a
    }
    
    return res
}

// nも浮動小数点に対応した版
fun power2(a: Double, n: Double) = if (a < 0 || n < 0) {
    println("inputs must be positive. return 0")
    0.0
} else a.pow(n)

fun main(args: Array<String>) {
    println(power2(2.0, 3.0))    // 8.0
    println(power2(4.5, 6.7))    // 23797
    println(power2(-1.0, 2.0))   // inputs must be positive. return 0 -> 0.0
}
```

