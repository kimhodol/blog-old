# sealed class

```kotlin
package dev.hodol.sandbox.base

sealed class Payment {
    class Cash(val value: Int): Payment()
    class Card(val no: Long): Payment()
}

fun eval(payment: Payment) =
    when (payment) {
        is Payment.Cash -> payment.value
        is Payment.Card -> payment.no
    }
```

* 상위 클래스에 `sealed` 키워드를 붙임으로써 하위 클래스 정의를 제한할 수 있다.
* `sealed class`를 사용하면 `when` 분기에서 `else` 구문을 생략할 수 있다. `enum`처럼 작동한다고 생각하자.
