# nested class와 inner class

```java
package dev.hodol.sandbox.base;

public class CustomInteger {

    private int value;

    public int indexOf(int index) {
        return StaticCustomIntegerCache.VALUES[index];
    }

    public int getValue() {
        return value;
    }

    class CustomIntegerCache {

        public int getCustomIntegerValue() {
            return value;
        }
    }

    static class StaticCustomIntegerCache {
        private static int[] VALUES = {1, 2, 3, 4, 5};
    }
}
```

* Java에서 **inner class**(`class` 내부의 `class`)는 outer class의 참조를 저장한다. 그래서 `CustomInteger`의 `value`를 `CustomIntegerCache`에서 **사용할 수 있다.**
* Java에서 **nested class**(`class` 내부의 `static class`)는 outer class의 참조를 저장하지 않는다. 그래서 `StaticCustomIntegerCache`에서는 `CustomInteger`의 `value`를 **사용할 수 없다.**

```kotlin
package dev.hodol.sandbox.base

class CustomInteger {
    val value = 0
    fun indexOf(index: Int): Int {
        return StaticCustomIntegerCache.VALUES[index]
    }

    inner class CustomIntegerCache {
        val customIntegerValue = value
        // val customIntegerValue = this@CustomInteger.value
    }

    class StaticCustomIntegerCache {
        companion object {
            val VALUES = intArrayOf(1, 2, 3, 4, 5)
        }
    }
}
```

* Kotlin에서는 Java 와 반대로 작동한다.&#x20;
  * Java의 inner class는 inner class며, nested class(static class)는 Kotlin에서 class다.
* Kotlin에서 **inner class**(`class` 내부의 `inner class`)는 outer class의 참조를 저장한다. **nested class**(`class` 내부의 `class`)는 outer class의 참조를 저장하지 않는다.
  * Kotlin에서 `inner class`는 `this@CustomInteger` 형식으로 외부 클래스를 참조할 수 있다.
  * Outer class에서도 inner class나 nested class의 `private`, `protected` 멤버에는 접근할 수 없다.

