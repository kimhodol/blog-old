# Tasks

[이전 글](https://woonjangahn.gitbook.io/devlog/kotlin/gradle/build-script)에 이어 Task에 대해 조금 더 자세히 알아보자.

### Task 실행 시 결과 라벨

Spring 프로젝트에서 `./gradlew build`를 실행하면 다음과 같이 출력된다. Task :\<task 이름> \<task 결과 라벨>가 출력되는데, task 결과 라벨에 대해 알아보자.

```bash
18:48:24: Executing task 'build'...

> Task :compileKotlin UP-TO-DATE
> Task :compileJava NO-SOURCE
> Task :processResources UP-TO-DATE
> Task :classes UP-TO-DATE
> Task :bootJarMainClassName UP-TO-DATE
> Task :bootJar UP-TO-DATE
> Task :inspectClassesForKotlinIC UP-TO-DATE
> Task :jar UP-TO-DATE
> Task :assemble UP-TO-DATE
> Task :compileTestKotlin UP-TO-DATE
> Task :compileTestJava NO-SOURCE
> Task :processTestResources NO-SOURCE
> Task :testClasses UP-TO-DATE
> Task :test
> Task :check
> Task :build
```

* 라벨이 없거나 or `EXECUTED`
  * Gradle이 task를 정상적으로 실행했다.
  * 또는 task 안에 아무 action이 없다.
* `UP-TO-DATE`
  * task의 output과 input이 변경되지 않았다.
* `FROM-CACHE`
  * task의 output을 이전 실행 기록에서 찾을 수 있었다.
* `SKIPPED`
  * task 실행이 제외되었다.
*   `NO-SOURCE`

    * Task가 실행되기 위해 필요한 소스파일이 존재하지 않는다.



### Task 정의하기

```kotlin
tasks.register("hello") {
    doLast {
        println("hello")
    }
}

tasks.register<Copy>("copy") {
    from(file("srcDir"))
    into(buildDir)
}

val hello2 by tasks.registering {
    doLast {
        println("hello")
    }
}

val copy2 by tasks.registering(Copy::class) {
    from(file("srcDir"))
    into(buildDir)
}

println(tasks.named("hello").get().name)
println(tasks.build.get().name)
println(tasks.named<Copy>("copy").get().destinationDir)
```

* `tasks.register(task 이름)`을 통해 task를 정의할 수 있다.
* `tasks.register<task type>(...)`을 통해 다른 task API를 활용할 수 있다. 예시에서는 [Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html) task를 사용해서 본문에서 이 API를 사용하고 있다.
* Kotlin의 [delegate properties](https://kotlinlang.org/docs/reference/delegated-properties.html)를 활용해서 task reference를 더 쉽게 할 수 있다. (`register` 대신 `registering` 사용)
* task 객체에 직접 접근하기 위해서 `task.named(...)`를 사용하거나, plugin에서 이미 정의된 경우 `tasks.<task 이름>`으로 접근할 수 있다.

### 서로 다른 프로젝트(모듈) task끼리 의존하기

```kotlin
project("project-a") {
    tasks.register("taskX") {
        dependsOn(":project-b:taskY")
        doLast {
            println("taskX")
        }
    }
}

project("project-b") {
    tasks.register("taskY") {
        doLast {
            println("taskY")
        }
    }
}

val taskX by tasks.registering {
    doLast {
        println("taskX")
    }
}

val taskY by tasks.registering {
    doLast {
        println("taskY")
    }
}

taskX {
    dependsOn(taskY)
}
```

### task 순서 조정하기

```kotlin
val taskX by tasks.registering {
    doLast {
        println("taskX")
    }
}
val taskY by tasks.registering {
    doLast {
        println("taskY")
    }
}
taskY {
    mustRunAfter(taskX)
    // shouldRunAfter(taskX)
}
```

* task끼리의 의존보다 단순히 순서를 조정하고 싶을 때는 `mustRunAfter(...)`, `shouldRunAfter(...)`를 사용한다.

### task 주석 붙이기

```kotlin
tasks.register<Copy>("copy") {
   description = "Copies the resource directory to the target directory."
   from("resources")
   into("target")
   include("**/*.txt", "**/*.xml", "**/*.properties")
}
```

### task 건너뛰기

```kotlin
val hello by tasks.registering {
    doLast {
        println("hello world")
    }
}

hello {
    onlyIf { !project.hasProperty("skipHello") }
}
```

### task disable 하기

```kotlin
val disableMe by tasks.registering {
    doLast {
        println("This should not be printed if the task is disabled.")
    }
}

disableMe {
    enabled = false
}
```

### [Incremental Build](https://docs.gradle.org/current/userguide/more\_about\_tasks.html#sec:up\_to\_date\_checks) (커스텀 Task 만들기)
