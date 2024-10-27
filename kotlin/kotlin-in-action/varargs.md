# varargs, 구조 분해 선언

```kotlin
package dev.hodol.sandbox.base

fun main(args: Array<String>) {
    val list = listOf(*args)
    println("$list")
}
```

* Java의 Varargs (`String...`) 과 같이 사용할 수 있다.
* 감싸져있는 Collection 객체를 걷어내고 내부 값만 사용할 수 있다는 점에 있어 JavaScript의 [Spread Operator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread\_syntax)과 유사하다.

```kotlin
package dev.hodol.sandbox.base

fun main(args: Array<String>) {
    val (k, v) = Pair("one", 1)
    println("$k: $v")
    
    val map = mapOf("one" to 1, "two" to 2)
    for ((key: String, value: Int) in map) {
        println("$key: $value")
    }
    
    for ((index, value) in listOf(1, 2, 3).withIndex()) {
        println("$index: $value")
    }
}
```

* _(실제로는 `class` 내부에 `componentN` 함수가 있다면)_ `Pair`, `Map`, `List.withIndex()` 등을 이용해 구조 분해 선언을 할 수 있다.&#x20;
* JavaScript의 [구조 분해 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring\_assignment)과 유사하다.
* Kotlin에서는 정규식 관련 함수를 많이 제공하는데, Group에도 구조 분해 선언을 활용할 수 있다.

```kotlin

package dev.hodol.sandbox.base

fun printParsedPath(value: String) {
    val regex = """(.+)/(.+)\.(.+)""".toRegex()
    val result = regex.matchEntire(value)
    if (result != null) {
        val (directory, filename, extension) = result.destructured
        println("dir: $directory, filename: $filename, extension: $extension")
    }
}

fun main() {
    printParsedPath("/Users/hodol/IdeaProjects/sandbox/docs.adoc")
}
```

