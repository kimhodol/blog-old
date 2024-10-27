# 중위(infix) 함수

```kotlin
package dev.hodol.sandbox.base

infix fun String.emphasize(mark: Char): String {
    return "$this $mark$mark"
}

fun main() {
    val map = mapOf("one" to 1, "two" to 2)
    println(map)
    
    println("Hello world".emphasize('!'))
    println("Hello world" emphasize '!')
}

```

* 중위(infix)함수는 인자가 하나뿐인 일반 메소드나 확장 함수에 사용할 수 있다. 함수 선언부 앞에 `infix` 키워드를 추가해서 호출할 수 있는데, `Map`이나 `Pair`에서 사용하는 `to`가 대표적인 중위함수다.
