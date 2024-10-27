---
description: Spring을 다루면서 Gradle을 크게 중요하게 생각해본적은 없지만, 이번 기회에 한 번 정리해보자.
---

# 소개

### 개요

[Gradle](https://gradle.org/)은 유연성과 성능을 중시하는 오픈소스 **빌드 자동화(소스 코드 컴파일, 패키징, 작업 자동화)** 툴이다. Java나 Kotlin을 하게 되면 어쩔 수 없이 만날 수 밖에 없는 툴인데, 비슷한 빌드 툴로는 Apache Ant나 Apache Maven이 있다.

![Maven이 확실히 강세지만 2010년대부터의 Gradle의 성장세도 눈에 띈다..](<../../.gitbook/assets/Screen Shot 2021-05-22 at 1.04.25.png>)

### Gradle의 특징

* High performance
  * input이나 output이 바뀌지 않는다면 Task_(작업 단위)_를 스킵한다.
  * build cache를 이용해 Task의 output을 다른 Tasks에서 사용할 수 있다.
  * 이 외에도 여러 최적화 방법이 있다.
* JVM foundation
  * Gradle은 JVM 위에서 작동하기 때문에 JDK가 필요하다.
  * Java API를 사용해서 build logic을 작성할 수 있다.
  * 물론 JVM뿐 아니라 여러 종류 기반의 프로젝트를 생성할 수 있다.
* Conventions
  * Maven의 많은 부분을 참고해서 만들었기 때문에, 기존의 Maven의 컨벤션으로 쉽게 사용할 수 있다.
  * 물론 override할 수 있다.
* Extensibility
  * Custom한 Task 또는 build model를 작성할 수 있다.
* IDE support
  * Android Studio, IntelliJ IDEA, Eclipse, NetBeans 등 여러 IDE에서 지원한다.
* Insight
  * Build scans를 통해 빌드 시에 어떤 이슈가 있는지 확인할 수 있고, 문제를 찾아 해결하거나 성능을 향상하는데 도움이 된다.

### Gradle에 대해 알아야 하는 5가지

* 범용적 목적을 가진 빌드 툴이다.
  * JVM 뿐 아니라 어떤 프로젝트던 만들 수 있다.
  * 단, 의존성 관리의 경우 Maven 또는 Ivy 호환 repositories, filesystem만 사용할 수 있는 제약이 있다.
* 중심이 되는 Gradle model은 Task를 중심으로 작성된다.
  * Gradle model은 Task들로 이루어진 Directed Acyclic Graphs이므로 의존성을 기준으로 여러 Task들을 묶어 연결할 수 있다.&#x20;
    * Gradle은 이 Graph를 기반으로 어떤 Task를 어떤 순서로 실행할지 결정한다.
  * Task는 Actions, Inputs, Outputs로 이루어진다.
    * Actions: 복사를 한다던지, 컴파일을 한다던지의 실제 작업
    * Inputs: Actions 수행에 사용될 값, 파일 또는 디렉토리
    * Outputs: Actions의 결과로 만들어지거나 수정되는 파일 또는 디렉토리
* 3가지의 Build Phase로 이루어진다.
  * Initialization: 빌드 환경을 설정하고 어떤 프로젝트들이 사용될지 결정된다.
  * Configuration: Task graph를 생성하고 설정하여 사용자가 설정한 것을 기반으로 Task들을 정렬한다.
  * Execution: Tasks들을 실행한다.
  * 이를 통해, 명령형보단 선언형의 방식을 택해 Configuration 단계에서 만들어진 코드가 execution 때 어떻게 변하는지 신경쓰지 않아도 된다.
* Build script는 여러 방법으로 확장 가능하다.
  * 커스텀하게 task type, task actions, extra properties, conventions, model 등을 만들 수 있다.
* API에 기반해 실행된다.
  * API에 기반해 실행되므로, build script가 실제로 어떻게 작동하는지 API 문서를 통해 확인할 수 있다.

