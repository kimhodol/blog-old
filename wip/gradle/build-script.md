# Build script

모든 Gradle build는 하나 이상의 project들로 이루어진다. 프로젝트의 결과는 library JAR이 될 수도, 웹 앱이 될 수도 있다. 또는 다른 프로젝트들의 결과물들을 모은 ZIP이 될 수도 있다.

project는 하나 이상의 task들로 이루어지는데, task는 build가 실행하는 atomic 단위의 작업이다. 클래스 컴파일이 될 수도 있고, JAR 생성이 될 수도 있고, JavaDoc을 생성하거나 저장소에 배포하는 것이 될 수도 있다. 일반적으로 task는 적용된 plugin들에서 정의한 것을 쓰게 되지만, 직접 생성할 수도 있다.

> `./gradlew <task이름>` 또는 (전역 설치가 되어 있는 경우) `gradle <task이름>`으로 단일 task를 실행할 수 있다.

### 첫번째 build script 작성하기

```kotlin
// build.gradle.kts
tasks.register("hello") {
    doLast {
        println("Hello world!")
    }
}
```

```bash
./gradlew -q hello
Hello world!
```

> `-q` 옵션은 Gradle의 로그성 메세지들을 제외하고 결과물만 보여주는 옵션이다. Quietly!
>
> `doLast`는 task가 실행될 때만 실행되도록 설정하는 scope며, 이것 없이 코드를 작성한다면 task가 실행되지 않더라도 build 시마다 코드가 실행된다.

`hello`라는 task를 정의하고, `doLast` 블록에 Hello world!를 출력할 코드를 넣었다.

### Kotlin을 활용해 task 만들기

```kotlin
tasks.register("upper") {
    doLast {
        val someString = "mY_nAmE"
        println("Original: $someString")
        println("Upper case: ${someString.toUpperCase()}")
    }
}
```

```bash
./gradlew upper -q
Original: mY_nAmE
Upper case: MY_NAME
```

이번엔 `upper`라는 task를 작성하고, Kotlin에서 쓰듯이 변수를 선언하고 이를 이용해 텍스트를 출력한다. 이렇게 그냥 Kotlin 코드를 써서 어떤 task의 내용을 작성할 수 있다.

### Task끼리 의존하기

```kotlin
tasks.register("hello") {
    doLast {
        println("Hello world!")
    }
}

tasks.register("intro") {
    dependsOn("hello")
    doLast {
        println("I'm Gradle")
    }
}
```

```bash
./gradlew -q intro
Hello world!
I'm Gradle
```

`dependsOn`을 활용해서 task로 하여금 다른 task를 먼저 실행시키고 실행되게 할 수 있다. 예를 들어 `문서를 만들고` → `문서를 옮기기` 가 그 예시가 될 수 있다.

### 여러 Task 만들기 / Task 확장하

```kotlin
repeat(4) { counter ->
    tasks.register("task$counter") {
        doLast {
            println("I'm task number $counter")
        }
    }
}
tasks.named("task0") { dependsOn("task2", "task3") }
```

```bash
./gradlew -q task0
I'm task number 2
I'm task number 3
I'm task number 0
```

Kotlin DSL를 이용해 task를 dynamic하게 만들 수 있으며, `named()`를 통해 기존 task를 확장할 수 있다.

### defaultTasks

```kotlin
defaultTasks("clean", "run")

task("clean") {
    doLast {
        println("Default Cleaning!")
    }
}

tasks.register("run") {
    doLast {
        println("Default Running!")
    }
}

tasks.register("other") {
    doLast {
        println("I'm not a default task!")
    }
}
```

```bash
./gradlew -q
Default Cleaning!
Default Running!
```

`defaultTask(...)`를 이용해 task 이름 없이 기본으로 실행될 task들을 정의할 수 있다.
