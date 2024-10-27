# 로컬(중첩) 함수

```kotlin
package dev.hodol.sandbox.base

class User (
    val name: String,
    val email: String,
)

fun User.validate() {
    fun validate(value: String, fieldName: String) {
        if (value.isEmpty()) {
            throw IllegalArgumentException("${fieldName}은 빈 값일 수 없습니다.")
        }
    }
    validate(name, "이름")
    validate(email, "이메일")
}

fun save(user: User) {
    user.validate()
    TODO("사용자를 저장한다.")
}
```

* Kotlin에서 함수/메소드는 중첩할 수 있다.
  * 물론 중첩 깊이는 최대 한 단계만을 권장한다.
* 확장함수와 로컬함수 등을 이용해 가독성을 높일 수 있다.
