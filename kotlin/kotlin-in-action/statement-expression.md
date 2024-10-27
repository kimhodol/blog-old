# Statement와 Expression

```kotlin
fun max(a: Int, b: Int): Int {
    return if (a > b) a else b
}

fun min(a: Int, b: Int): Int = if (a < b) a else b
```

> Kotlin에서 if문은 Expression이지 Statement가 아니다. Expression은 값을 만들어내며, 다른 식의 하위 요소로 계산에 참여할 수 있는 반면 Statement는 자신을 둘러 싸고 있는 가장 안쪽 블록의 최상위 요소로 존재하며 아무런 값을 만들어내지 않는 다는 차이가 있다. Java에서는 모든 제어 구조가 Statement인 반면 Kotlin에서는 Loop를 제외한 대부분의 제어 구조가 Expression이다. 제어구조를 다른 식으로 엮어낼 수 있으면 여러 일반적인 패턴을 아주 간결하게 표현할 수 있다. 반면, 대입문은 Java에서는 Statement였으나 Kotlin에서는 Expression이 되었다. 그로 인해 Java와 달리 대입식과 비교식을 잘못 바꿔 서서 버그가 생기는 경우가 Kotlin에는 없다.

```kotlin
fun eval3(e: Expr): Int {
    if (e is Num) {
        e.value
    } else if (e is Sum) {
        eval(e.right) + eval(e.left)
    } else {
        throw IllegalArgumentException("Unknown expression")
    }
}
```

* `return`문이 없어도 마지막에 나오는 expression을 `return`한다.
