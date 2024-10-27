# class와 inheritance

```kotlin
package dev.hodol.sandbox.base

interface Monster {
    fun eat()
    fun sleep()
}

abstract class Mammal {
    abstract fun cry()
    abstract fun run()
    fun walk() = println("터벅터벅")
}

open class Pig(val name: String, open val power: Int) : Monster, Mammal() {
    fun attack() = println("${name}이 공격")
    open fun dance() = println("춤을 추자")
    override fun eat() = println("냠냠")
    final override fun sleep() = println("쿨쿨")
    final override fun cry() = println("엉엉")
    override fun run() = println("달려라")
}

class RibbonPig(
    name: String,
    override val power: Int,
    private val color: String
) : Pig(name, power) {
    override fun dance() {
        withRibbon()
        super.dance()
    }

    override fun eat() {
        withRibbon()
        super.eat()
    }

    override fun run() {
        withRibbon()
        super.run()
    }

    private fun withRibbon() {
        print("${color}색 리본을 달고 ")
    }
}
```

* `interface`를 구현할 때는 괄호가 없어야하고, `class`를 상속할 때는 괄호가 있어야한다.
* `class`는 기본적으로 `final`이며, 상속을 위해서 `open` 키워드가 필요하다.
  * 상위 객체의 생성자를 사용해야하며 이 때 프로퍼티 역시 기본적으로 `final`이다.
  * 메소드 역시 기본적으로 `final`이므로 하위 객체에서 `override`하고 싶다면 `open` 키워드를 붙이자.
* Kotlin의 `abstract class`는 Java와 유사한데, 추상 메소드에 한해 `abstract` 키워드를 붙이며 반드시 구현해야 한다.
  * `abstract` 메소드는 반드시 `override` 해야하므로 당연히 `open` 상태이다. 하위 객체에서 `override`를 금지시키기 위해선 `final` 키워드가 필요하다.
* 물론, [상속보단 조합](https://en.wikipedia.org/wiki/Composition\_over\_inheritance).
