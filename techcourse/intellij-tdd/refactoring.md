# 리팩토링

### 리팩토링의 시작

지난 글에서 우리는 `new Number(1).getValue()` 가 `1` 과 일치하는지에 대한 테스트를 작성했다. 이제 TDD의 세번째 단계인 리팩토링을 통해 우리의 요구사항에 맞는 코드를 작성해보자. 우리가 정리한 요구사항에 따른 테스트는 다음과 같았다.

* 1\~100 사이의 임의의 정수 생성
  * 임의의 값을 생성할 때 범위를 넘어가면 Exception을 throw할 것

#### 1\~100 사이의 수를 넣었을 때 `getValue()` 가 작동할 것인가?

```java
// src/test/java/RandomNumberTest.java

import static org.assertj.core.api.Assertions.*;

import org.junit.Test;

public class RandomNumberTest {
    @Test
    public void 생성된_숫자가_범위를_벗어나지_않는지() {
        RandomNumber randomNumber = new RandomNumber(1);
        assertThat(randomNumber.getValue()).isEqualTo(1);
        RandomNumber ten = new RandomNumber(10);
        assertThat(ten.getValue()).isEqualTo(10);
        RandomNumber hundred = new RandomNumber(100);
        assertThat(hundred.getValue()).isEqualTo(100);
    }
}
```

리팩토링 전에 간단한 코드를 몇 개 추가해보자. 인자로 `10` 또는 `100` 을 넣었을 때도 `getValue()` 가 제대로 된 값을 가져올까?

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_18.59.22.png)

예상한대로 항상 1을 `return`하고 있는 지금의 `getValue()`의 테스트는 실패한다. 성공하기 위해선 어떻게 해야할까?

```java
// src/main/java/RandomNumber.java

public class RandomNumber {
    private int number;
    public RandomNumber(int number) {
        this.number = number;
    }

    public int getValue() {
        return number;
    }
}
```

`RandomNumber` 내에 `number` 라는 인스턴스 변수를 선언하고 생성자 단에서 인자를 대입하기로 했다. 또한 `getValue()` 에서 이 인스턴스 변수인 `number` 를 반환하기로 했다. 테스트 해보자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_19.05.24.png)

다행히도(?) 성공했다. 그럼 이제 기분 좋게 다음 요구사항으로 넘어가자.

### 범위를 넘어가면 `Exception`을 `throw`할 것

우리는 TDD의 한 사이클을 돌고 다시 원점으로 돌아왔다. 그렇다면 이제 해야할 일은?

#### 1. Test가 실패하는 코드를 작성한다.

우리는 `new RandomNumber()`의 인자로 `0` 또는 `101`이 들어왔을 때 `IllegalArgumentException` 이 나와야 한다고 생각하니, 그대로 작성해보자. `Exception` 을 던지는지 확인하는 테스트는 다음과 같이 작성한다.

```java
// src/test/java/RandomNumberTest.java

import static org.assertj.core.api.Assertions.*;

import org.junit.Test;

public class RandomNumberTest {
    @Test
    public void 생성된_숫자가_범위를_벗어나지_않는지() {
        RandomNumber randomNumber = new RandomNumber(1);
        assertThat(randomNumber.getValue()).isEqualTo(1);
        RandomNumber ten = new RandomNumber(10);
        assertThat(ten.getValue()).isEqualTo(10);
        RandomNumber hundred = new RandomNumber(100);
        assertThat(hundred.getValue()).isEqualTo(100);

        assertThatThrownBy(() -> {
            RandomNumber zero = new RandomNumber(0);
        }).isInstanceOf(IllegalArgumentException.class)
          .hasMessage("1부터 100 이하의 숫자만 가능합니다.");

        assertThatThrownBy(() -> {
            RandomNumber hundredOne = new RandomNumber(101);
        }).isInstanceOf(IllegalArgumentException.class)
          .hasMessage("1부터 100 이하의 숫자만 가능합니다.");
    }
}
```

`assertThatThrownBy` 는 인자로 익명의 메소드를 받게 되는데, 이 메소드 내에서 `Exception` 을 `throw` 한다면 `isInstanceOf()` 의 인자에 `Exception`의 클래스를 넣어 일치한다면 테스트를 통과시킨다. `hasMessage()` 는 이 `Exception`이 `throw` 될 때 특정 메세지를 가지고 나오는지 확인한다. 테스트를 실행해보자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_19.16.33.png)

당연히 **Tests failed**가 뜨며, 우리는 성공적으로 또 다른 사이클의 1단계를 성공적으로 진행했다. 이제 2단계를 진행하자.

#### 2. Test를 성공할 수 있는 최소한의 Production 코드를 작성한다.

```java
// src/main/java/RandomNumber.java

public class RandomNumber {
    private int number;

    public RandomNumber(int number) {
        if (number == 0 || number == 101) {
            throw new IllegalArgumentException("1부터 100 이하의 숫자만 가능합니다.");
        }
        this.number = number;
    }

    public int getValue() {
        return number;
    }
}
```

인자로 `0` 또는 `101` 이 들어온다면 `RandomNumber` 의 생성자에서 `IllegalArugmentException`을 `throw` 하도록 코드를 작성했다. 그렇다면 테스트를 돌려보자.

![](../../.gitbook/assets/Screen\_Shot\_2020-02-17\_at\_19.21.03.png)

성공적으로 테스트가 통과했다!

#### 3. 작성한 Production 코드를 리팩토링한다.

이제 다시 3단계로 돌아왔다. `0` 또는 `101` 뿐 아니라 어떤 숫자가 들어와도 `IllegalArugmentException`을 `throw` 하기 위해 리팩토링 해보자.

```java
public class RandomNumber {
    private int number;

    public RandomNumber(int number) {
        if (number < 1 || number > 100) {
            throw new IllegalArgumentException("1부터 100 이하의 숫자만 가능합니다.");
        }
        this.number = number;
    }

    public int getValue() {
        return number;
    }
}
```

`number` 가 단순히 `0`이나 `101`과 일치할 뿐만 아니라, `1` 보다 크거나 `100` 보다 크면 `Exception` 을 `throw` 하도록 변경했다.

### 이제 슬슬 감이 잡힐텐데

우리는 성공적으로 두 번의 사이클을 돌았다.

1. 실패하는 테스트 코드를 작성하고
2. 테스트가 성공하도록 최소한의 프로덕션 코드를 구현하고
3. 이를 리팩토링한다.

이런 방식으로 테스트 코드를 먼저 짜고 이를 충족하는 프로덕션 코드를 짠다면, 결과적으로 프로덕션 코드는 항상 테스트에 통과한 상태의 코드일 것이며, 요구사항이 변하거나 비즈니스 로직에 변경이 있을 때도 테스트를 통해 쉽게 어디에 문제가 있는지 알 수 있다. 다음 글에서는 **테스트 코드 역시 유지 보수 가능해야한다**는 것을 명심하며 테스트 코드를 리팩토링 해보자.
