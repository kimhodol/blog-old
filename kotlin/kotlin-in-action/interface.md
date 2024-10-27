# interface

```kotlin
package dev.hodol.sandbox.base

interface Movable {
    fun move(step: Int)
    fun stop() = println("멈춰!")
}

interface Walkable {
    fun move(step: Int)
    fun stop() = println("걸음을 멈춰!")
}

class Human: Movable, Walkable {
    override fun move(step: Int) {
      println("${step}만큼 움직여!")
    }

    override fun stop() {
        super<Movable>.stop()
    }
}
```

* Kotlin에서 `interface`는 Java와 유사하다.
* `default` 키워드 없어도 기본 메소드를 작성할 수 있다.
* `override` 키워드는 강제된다.
* `super`같은 경우는 `Movable.super.stop()` 대신 `super<Moveable>.stop()` 처럼 작성한다.

