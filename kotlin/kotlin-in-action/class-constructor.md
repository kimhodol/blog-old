# class 생성자

```kotlin
package dev.hodol.sandbox.base

class User(_name: String) {
    val name: String

    init {
        name = _name
    }
}

class User2(_name: String) {
    val name = _name
}

class User3(val name: String)
```

* `init` 블록을 이용해 클래스 초기화 로직을 담을 수 있다.
* 클래스 상단에서 생성자 인자를 이용해 프로퍼티를 초기화할 수 있다.
* 생성자 인자에 `val`, `var`이 붙으면 프로퍼티가 된다.

```kotlin
package dev.hodol.sandbox.base

open class User(val name: String)

class FacebookUser(name: String, id: Long): User(name)

class TwitterUser: User {
    constructor(name: String, id: Long): super(name) {
        // ...
    }

    constructor(name: String): this(name, 0)
}
```

* 하위 클래스에서 상위 클래스의 생성자 인자를 사용할 수 있다.
*   `constructor`에서 초기화 해도 되며, `super`이나 `this`가 Java에서 메소드 블록 최상단에 있었다면, Kotlin에서는 `constructor(...): super(...)`, `constructor(...): this(...)` 과 같은 형식으로 생성을 위임할 수 있다.

    &#x20;
