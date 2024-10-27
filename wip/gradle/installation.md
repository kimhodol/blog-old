# 설치 및 프로젝트 생성

Gradle 프로젝트를 생성하는 2가지를 알아보자.

### 1. (추천) IDE의 힘을 빌리기

IntelliJ의 New Project에서 Gradle Project를 선택할 수 있다.&#x20;

![여기서는 Kotlin DSL build script, Kotlin/JVM에 체크를 해주자.](<../../.gitbook/assets/Screen Shot 2021-05-22 at 1.15.15.png>)

![GroupId와 ArtifactId는 기본 디렉토리 구조를 만들 때 패키지 이름으로 사용된다.](<../../.gitbook/assets/Screen Shot 2021-05-22 at 1.16.58.png>)

### 2. Gradle을 전역 설치하여 프로젝트 생성하기

#### Homebrew를 사용해 gradle을 설치한다.

```bash
brew install gradle
gradle -v
```

#### 프로젝트 폴더를 생성하고 gradle 프로젝트를 init 한다.

```bash
mkdir demo
cd demo
gradle init

Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Scala
  6: Swift
Enter selection (default: Java) [1..6] 4

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2] 2

Project name (default: demo):
Source package (default: demo):


BUILD SUCCESSFUL
2 actionable tasks: 2 executed
```
