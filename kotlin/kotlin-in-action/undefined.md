# 확장 함수와 프로퍼티

```kotlin
package dev.hodol.sandbox.base

fun String.lastChar(): Char = this[this.length - 1]

var String.firstChar: Char
    get() = get(0)
    set(value: Char) {
        this.replaceFirst(this[0], value)
    }
```

* 기존에 존재하던 객체의 메소드나 프로퍼티처럼 사용할 수 있는 확장 함수와 확장 프로퍼티가 있다.
* 함수를 선언할 때 `객체.메소드명()` 또는 `val 객체.프로퍼티명`의 형식으로 선언할 수 있으며, 함수 본문에서의 `this`는 `객체`를 나타낸다.
  * 객체 멤버에 접근할 때 `this`는 생략할 수 있다.
* 확장 함수는 정적으로 결정된다. (어떤 타입으로 선언되었냐에 따라 달라진다)
* 확장 함수는 하위 객체로 상속되지 않는다. (오버라이드 할 수 없다)
* Java에서 사용할 시에는 일반적인 `static` 메소드처럼 사용된다. (이 경우 `StringUtilsKt.lastChar("Java");`)
