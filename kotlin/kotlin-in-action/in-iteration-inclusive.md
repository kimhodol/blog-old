# in 구문 (Iteration, Inclusive)

```kotlin
for (i in 1..100) {
    println(i)
}

for (c in 'A'..'F') {
    println(Integer.toBinaryString(c.toInt())
}

for (i in 100 downTo 1 step 2) {
    println(i)
}
```

* `1..100` 은 `1 <= i <= 100`의 범위를 의미한다.
  * 양끝 모두 폐구간임에 주목하자.
  * 숫자 타입뿐 아니라 `Char` 타입에도 사용할 수 있다.
* `1 <= i < 100`을 표현하고 싶다면 `1 until 100`을 사용하자.
* `downTo` 역시 `100 >= i >= 1`을 의미하며, `step`을 사용하면 증가값의 절대값이 변한다.
  * 위 코드의 경우엔 `100, 98, 96...`

```kotlin
val binaryReps = TreeMap<Char, String>()

for (c in 'A'..'F') {
    binaryReps[c] = Integer.toBinaryString(c.toInt()
}

for ((letter, binary) in binaryReps) {
    println("$letter = $binary")
}

for ((index, element) in list.withIndex()) {
    println("$index: $element")
}
```

* JavaScript의 [Destructuring](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring\_assignment)을 생각하면 편하다.
  * `binaryReps`의 각각의 쌍을 이루는 (`key`, `value`)를 풀어 사용할 수 있다.
* `List`를 순회하며 `index`를 사용하고 싶으면 `withIndex()`를 이용해 같은 원리로 사용할 수 있다.

```kotlin
fun isLetter(c: Char) = c in 'a'..'z' || c in  'A'..'Z'
fun isNotDigit(c: Char) = c !in '0'..'9'

println("Kotlin" in "Java".."Scala") // true
println("Kotlin" in setOf("Java", "Scala") // false
```

* `in` 구문을 활용해 반복문뿐 아니라 포함 여부를 확인할 수도 있다.
  * `!in`은 포함되지 않는다는 표현이다.
  * 반복문에서는 `String` 범위를 사용할 수는 없지만, 포함 여부를 확인할 때는 알파벳 순으로 범위 안에 속하는지 확인 할 수 있다. 단, 일치를 원한다면 Collection을 사용하자.
