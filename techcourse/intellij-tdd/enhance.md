# 테스트 코드 유지보수 하기

지난 글까지 우리는 TDD를 처음 접하는 입장에서 Java와 IntelliJ를 이용해 테스트를 작성해보고 이를 이용해 실제로 TDD가 어떻게 이루어지는지 경험해봤다. 그렇다면 테스트 코드도 짰고, 프로덕션 코드도 문제 없이 작동하고 있으니 이제 끝일까?

### 테스트 코드 역시 유지보수의 대상이다

#### 작은 단위로 나눠보자

지금까지 썼던 테스트 코드를 보자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-18\_at\_19.55.24.png)

`assertThat` 과 `assertThatThrownBy` 가 난무하고 있는데, 더 복잡한 어플리케이션에서는 테스트 코드가 하염 없이 길어질 수 밖에 없다. 이를 쪼개보고, 더 적절한 이름들을 붙여보자.

```java
import static org.assertj.core.api.Assertions.*;

import org.junit.jupiter.api.Test;

public class RandomNumberTest {
    @Test
    public void 최소값을_넣었을때_성공() {
        RandomNumber randomNumber = new RandomNumber(1);
        assertThat(randomNumber.getValue()).isEqualTo(1);
    }

    @Test
    public void 적당한_숫자를_넣었을때_성공() {
        RandomNumber ten = new RandomNumber(10);
        assertThat(ten.getValue()).isEqualTo(10);
    }

    @Test
    public void 최대값을_넣었을때_성공() {
        RandomNumber hundred = new RandomNumber(100);
        assertThat(hundred.getValue()).isEqualTo(100);
    }

    @Test
    public void 최소값보다_작은_수를_넣으면_예외발생() {
        assertThatThrownBy(() -> {
            RandomNumber zero = new RandomNumber(0);
        }).isInstanceOf(IllegalArgumentException.class)
            .hasMessage("1부터 100 이하의 숫자만 가능합니다.");
    }

    @Test
    public void 최대값보다_큰_수를_넣으면_예외발생() {
        assertThatThrownBy(() -> {
            RandomNumber hundredOne = new RandomNumber(101);
        }).isInstanceOf(IllegalArgumentException.class)
            .hasMessage("1부터 100 이하의 숫자만 가능합니다.");
    }
}
```

여러 메소드가 있는 경우, 이 메소드들이 있는 클래스 왼쪽 실행 버튼을 누르거나, 메소드명에 커서를 두고 단축키(MacOS의 경우 `⌃⇧R`)를 누르면 모든 메소드 실행이 가능하다. 적당한 메소드명과 함께 경우를 나눠보니, 테스트 결과가 비교적 더 깔끔하게 나오고, 프로그래머 입장에서 읽기도 쉽다.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-18\_at\_20.29.00.png)

메소드명을 한글로 지으니 보기도 좋다. 외국인 팀원이 있는 것이 아니라면, 테스트명을 한글로 짓기로 팀에서 합의를 보는 것은 어떨까?

#### 필요가 적은 경우는 없애보자

```java
@Test
public void 최소값을_넣었을때_성공() {
    RandomNumber randomNumber = new RandomNumber(1);
    assertThat(randomNumber.getValue()).isEqualTo(1);
}

@Test
public void 적당한_숫자를_넣었을때_성공() {
    RandomNumber ten = new RandomNumber(10);
    assertThat(ten.getValue()).isEqualTo(10);
}

@Test
public void 최대값을_넣었을때_성공() {
    RandomNumber hundred = new RandomNumber(100);
    assertThat(hundred.getValue()).isEqualTo(100);
}
```

위 세가지 경우는 모두 예외가 발생하지 않곡 성공하는 경우인데, 그렇다면 이런 생각이 들 수 있다.

> 그럼 2는? 3은? 4는? 98은? 99는?

테스트를 짤 때 어디부터 어디까지 넣는 것이 좋을까? 사람들마다 의견이 다를 수 있지만, **1부터 100**이라는 어떤 범위 내의 것만 작동한다는 것을 테스트하고 싶다면 최소값과 최대값, 그리고 그 값들을 간신히 넘어서는 값들(이 경우에는 **0과 101**) 정도를 테스트하는 것이 실용적이라고 할 수 있다. 또한, 이런 값들을 `static final` 키워드를 이용해 상수화하여 대신 넣는다면 더 보기 좋은 코드가 될 것이다. 위 경우에서 우리는 `10` 이라는 애매한 값에 대한 테스트를 제외하고 `1` 과 `100` 이라는 경계 케이스(Boundary case, Edge case) 정도만 남겨두면 될 것 같다. 이제 우리 코드는 다음과 같다.

```java
import static org.assertj.core.api.Assertions.*;

import org.junit.jupiter.api.Test;

public class RandomNumberTest {

    public static final int MIN_NUMBER = 1;
    public static final int MAX_NUMBER = 100;
    public static final int LESS_THAN_MIN_NUMBER = 0;
    public static final int LARGER_THAN_MAX_NUMBER = 101;

    @Test
    public void 최소값을_넣었을때_성공() {
        RandomNumber randomNumber = new RandomNumber(MIN_NUMBER);
        assertThat(randomNumber.getValue()).isEqualTo(MIN_NUMBER);
    }

    @Test
    public void 최대값을_넣었을때_성공() {
        RandomNumber hundred = new RandomNumber(MAX_NUMBER);
        assertThat(hundred.getValue()).isEqualTo(MAX_NUMBER);
    }

    @Test
    public void 최소값보다_작은_수를_넣으면_예외발생() {
        assertThatThrownBy(() -> {
            RandomNumber zero = new RandomNumber(LESS_THAN_MIN_NUMBER);
        }).isInstanceOf(IllegalArgumentException.class)
            .hasMessage("1부터 100 이하의 숫자만 가능합니다.");
    }

    @Test
    public void 최대값보다_큰_수를_넣으면_예외발생() {
        assertThatThrownBy(() -> {
            RandomNumber hundredOne = new RandomNumber(LARGER_THAN_MAX_NUMBER);
        }).isInstanceOf(IllegalArgumentException.class)
            .hasMessage("1부터 100 이하의 숫자만 가능합니다.");
    }
}
```

#### `@ParameterizedTest` 를 이용해 중복을 제거하자

지금까지의 코드를 보면 예외를 발생시키지 않는 두 경우(1과 100), 발생시키는 두 경우(0과 101)에서 각각 코드의 중복이 보인다. 테스트 해야하는 경우들이 늘어나면 코드의 양은 기하급수 적으로 늘 것이다. 이는 `@ParameterizedTest` 를 이용해 중복을 제거하고 하나의 메소드에 인자를 넣어 가변적인 테스트 함수로 바꿀 수 있다. `@ParameterizedTest` 에는 가변적으로 변수를 주기 위해 `@ValueSource`, `@CsvSource`, `@MethodSource` 등을 함께 사용할 수 있는데, 우선 제일 기본적인 `@ValueSource` 를 사용해보고 나머지는 각자 공부해보자. 우선 수정된 코드는 다음과 같다.

```java
import static org.assertj.core.api.Assertions.*;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

public class RandomNumberTest {

    public static final int MIN_NUMBER = 1;
    public static final int MAX_NUMBER = 100;
    public static final int LESS_THAN_MIN_NUMBER = 0;
    public static final int LARGER_THAN_MAX_NUMBER = 101;

    @ParameterizedTest
    @ValueSource(ints = {MIN_NUMBER, MAX_NUMBER})
    public void 최소값_또는_최대값_넣었을때_성공() {
        RandomNumber randomNumber = new RandomNumber(MIN_NUMBER);
        assertThat(randomNumber.getValue()).isEqualTo(MIN_NUMBER);
    }

    @ParameterizedTest
    @ValueSource(ints = {LESS_THAN_MIN_NUMBER, LARGER_THAN_MAX_NUMBER})
    public void 범위_밖의_값을_넣으면_예외발생() {
        assertThatThrownBy(() -> {
            RandomNumber zero = new RandomNumber(LESS_THAN_MIN_NUMBER);
        }).isInstanceOf(IllegalArgumentException.class)
            .hasMessage("1부터 100 이하의 숫자만 가능합니다.");
    }
}
```

훨씬 깔끔하고 유지보수하기 쉬운 모양이 되지 않았는가? 이렇게 테스트 코드 역시 유지보수해야하는 코드라는 생각을 가지고 프로그래머가 읽기 쉬운 코드로 리팩토링 하는 것이 바람직하다.

### 테스트는 귀찮을 수도 있다

원래 프로덕션 코드만 쓰던 프로그래머들은 테스트 코드를 짜고 이를 바탕으로 비즈니스 로직을 구현하는 것에 익숙하지 않다. 하지만 테스트 코드를 쓰고 TDD를 몸에 익히기 시작하면 결국 테스트 없이는 프로덕션 코드를 짜는 것이 두려워질 만큼 테스트를 좋아하게 된다. 결국 TDD는 유지보수를 위해서도, 좀 더 쉬운 협업을 위해서도 쓰는 것이지만 결국 제일 중요한 것은 프로그래머가 자신의 코드에 자신감을 가지게 해준다는 것이 제일 큰 장점인것 같다.

#### 물론 TDD는 절대로 만능이 아니다

프로그래머들은 역사적으로 더 효율적이고 효과적인 개발을 하기 위해 많은 고민을 거듭하고 이를 통해 여러 방법론을 만들었다. TDD 역시 그 중 하나일 뿐이다. 어떤 방법론을 맹목적으로 신봉하고 무작정 따라하기보단, 자신의 상황과 팀의 상황을 충분히 고려하고 이 방법론을 도입하는 것이 어떤 이득과 손해를 가져다 줄지 충분히 생각하고 쓰는 것이 합당하다. 하지만 TDD는 굉장히 핫하고 많이 사용된 방법론이라는 것도 사실이다. 이번 시리즈의 글들은 비록 많이 부족하지만, Unit test나 TDD에 처음 입문하는 사람들에게 좋은 가이드가 될 수 있었으면 좋겠다. (끝)
