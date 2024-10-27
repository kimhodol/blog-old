# 첫 테스트 작성하기

### 첫 테스트를 짜보자

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_16.44.10.png)

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.31.51.png)

#### 사전 세팅

* Java 8 / IntelliJ 2019.3
* `New...` - `Project` - `Gradle` - Java만 체크하고 프로젝트 생성
* `build.gradle` 에 `testCompile('org.assertj:assertj-core:3.9.1');` 추가 후 `Import Changes` 선택

#### 프로덕션 코드와 테스트 코드

프로젝트의 디렉토리를 보면, `src` 하위에 `main` 과 `test` 폴더가 있는데, `main` 에는 실제 기능을 가지고 있는 프로덕션 코드(Production code)가 들어가며, `test` 에는 테스트를 위한 테스트 코드(Test code)가 들어간다. `main` 과 `test` 에서 테스트할 함수는 관습적으로 같은 각각 패키지 내에 있어야 한다. 즉, `src/main/java/domain/RandomNumber.java` 의 테스트는 `src/test/java/domain/RandomNumberTest.java` 와 같이 각각의 디렉토리 내 같은 패키지에 있어야 한다는 뜻이다.

#### 테스트 코드를 프로덕션 코드보다 먼저 작성한다

우선 `src/test/java/domain/RandomNumberTest.java` 를 작성하자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.06.01.png)

오른쪽 마우스 클릭, 또는 단축키(MacOS의 경우 `⌘N`)를 통해 `generate` 를 클릭하면 다음과 같이 메뉴가 뜨는데, 이는 `test` 폴더 내에 있는 파일이기 때문에 IntelliJ에서 제공해주는 기능이다. 여기서 `Test Method` 를 선택하여 첫번째 테스트를 만들어보자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.06.47.png)

`name` 의 자리에 `생성된_숫자가_범위를_넘어서지_않는지` 라고 썼는데, 메소드명 같은 경우 알아보기 쉽게 하기 위해 한글과 `_` 의 조합으로 작성하는 경우도 있다. (물론 이 대신에 `DisplayName` 을 이용해 메소드의 용도를 적기도 하는데, 이는 다음에 설명하겠다.) 이제 테스트를 짜보자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.09.35.png)

`RandomNumber` 은 아직 존재하지 않는 클래스이며, 인스턴스 메소드인 `getValue()` 역시 아직 존재하지 않는다. `assertThat()` 과 `isEqualTo()` 는 `org.assertj.core.api.Assertions.*` 를 `static` 하게 가져와서 사용하는 함수로, `assertThat()` 의 인자가 반환하는 값과 `isEqualTo()` 의 인자인 우리가 생각하는 기대값을 비교하여 테스트가 성공하는지 실패하는지 알 수 있다.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.12.36.png)

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.16.44.png)

아직 존재하지 않는 클래스인 `RandomNumber` 과 `getValue()` 를 만들어주기 위해 에러로 밑줄이 그어져 있는 각각에 오른쪽 클릭 또는 단축키(MacOS의 경우 `⎇⏎`)를 통해 `Show Context Actions` 를 선택하자. `Create Class 'RandomNumber'` 를 클릭하고 `Target destination Directory` 에서 프로덕션 코드가 들어가야 하는 `main` 내부의 같은 패키지를 선택하여 생성하자. 다시 테스트 코드로 돌아오면 `new RandomNumber()` 의 인자에 빨간 줄이 그어져 있는데, 이 역시 위와 같은 방법으로 우리가 원하는 `int` 형 인자를 받는 생성자를 만들어주자.

#### 첫번째 테스트 돌려보기

지금까지 잘 따라왔다면 테스트 코드(`RandomNumberTest`)와 프로덕션 코드(`RandomNumber`)가 각각 다음과 같은 상태일 것이다.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.37.04.png)

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.37.01.png)

`RandomNumberTest` 가 실행될 때 이제 컴파일 에러는 더 이상 나지 않는다. `RandomNumberTest` 에서 클래스명 왼쪽의 실행 버튼, 또는 단축키(MacOS의 경우 `⌃⇧R`)를 이용해 테스트를 실행해보자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.40.36.png)

여러분의 첫번째 테스트를 돌려봤다. **Test failed** 라고 빨간색으로 뜨니까 기분이 나쁘다. `getValue()` 의 반환값을 0이 아닌 1로 바꿔주자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_17.42.32.png)

기분 좋은 **Test passed**라는 문구와 함께 성공했다! 여러분의 테스트는 성공적으로 통과했다.

### 이게 무슨 짓이냐고?

1편에서 이야기한 TDD의 과정이 기억나는가?

1. Test가 실패하는 코드를 작성한다.
2. Test를 성공할 수 있는 최소한의 Production 코드를 작성한다.
3. 작성한 Production 코드를 리팩토링한다.

우린 이미 2번까지 진행한 것이다. 다음편에서는 좀 더 상식적인 테스트 성공을 위해 3번, 작동하는 코드 리팩토링을 진행해보자.
