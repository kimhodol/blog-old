# when 구문과 Smart Cast

```kotlin
inteface Expr
class Num(val value: Int): Expr
class Sum(val left: Expr, val right: Expr): Expr

fun eval(e: Expr): Int {
    when(e) {
        is Num -> e.value
        is Sum -> eval(e.right) + eval(e.left)
        else -> throw IllegalArgumentException("Unknown expression")
    }
}

fun eval2(e: Expr): Int {
    when {
        e is Num -> e.value
        e is Sum -> {
            eval(e.right) + eval(e.left)
        }
        else -> throw IllegalArgumentException("Unknown expression")
    }
}

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

* `when` 구문은 기존의 `switch`-`case` 구문을 대체할 수 있다.
  * `when` 뒤에 괄호가 없다면, 각각의 케이스는 `boolean`을 반환하는 식을 넣어야 한다.
  * Java의 `switch` 구문에는 primitive value와 String만을 사용할 수 있지만, Kotlin에서의 `when`은 임의의 객체를 허용한다.
* `is`는 Java의 `instanceof`와 비슷한데, 명시적으로 타입을 캐스팅하지 않아도 사용할 수 있다. (`e.right`, `e.left`)
* 분기는 블록형태로도 만들 수 있으며, `return`문이 없어도 마지막에 나오는 expression을 `return`한다.
  * 각각의 분기는 **한 개 이상**의 조건을 포함할 수 있다.
