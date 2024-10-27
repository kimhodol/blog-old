# 세상에서 제일 쉬운 MVC 패턴

### [MDN에서의 정의](https://developer.mozilla.org/ko/docs/Glossary/MVC)

> MVC (모델-뷰-컨트롤러) 는 사용자 인터페이스, 데이터 및 논리 제어를 구현하는데 널리 사용되는 소프트웨어 디자인 패턴입니다. 소프트웨어의 비즈니스 로직과 화면을 구분하는데 중점을 두고 있습니다. 이러한 "관심사 분리" 는 더나은 업무의 분리와 향상된 관리를 제공합니다. MVC 에 기반을 둔 몇 가지 다른 디자인 패턴으로 MVVM (모델-뷰-뷰모델), MVP (모델-뷰-프리젠터), MVW (모델-뷰-왓에버) 가 있습니다. MVC 소프트웨어 디자인 패턴의 세 가지 부분은 다음과 같이 설명할 수 있습니다.

* **모델**: 데이터와 비즈니스 로직을 관리합니다.
* **뷰**: 레이아웃과 화면을 처리합니다.
* **컨트롤러**: 명령을 모델과 뷰 부분으로 라우팅합니다.

### MVC는 왜 생긴걸까?

MVC 패턴은 소프트웨어를 개발할 때 Model, View, Controller 세 가지 형태로 역할을 나누어 개발하는 **디자인 패턴**이다.

디자인 패턴은 본래 건축학에서 사용되는 용어로, **설계 문제에 대한 해답을 문서화하기 위해** 고안된 방법이다. 컴퓨터 과학 등에서도 이 방식을 도입하였는데, 과거에 소프트웨어 개발 과정에서 맞닥뜨린 문제를 해결하고 그 노하우에 이름을 붙여 재이용하기 좋은 형태로 정리한 것이다. 이 과정에서 생긴 큰 장점이 생겼는데, 바로 패턴을 알고 있는 사람들끼리 소통하기 쉬워졌다는 것이다. 예를 들면 다음과 같다.

> **"쇼핑몰 프로젝트를 만들 때 고객, 페이먼트, 상품 등 객체로서 데이터베이스에 연결되어 사용되는 것들을 한 폴더에 묶고요, 이걸 페이지에 띄워줘야하는데, 그 페이지들의 템플릿을 한 폴더에 묶고요, 한 폴더에는 라우팅이 들어오면 이걸 어떤 객체를 어떤 템플릿에 넣어 띄워줄 지에 대한 로직들이 들어있어요."**

라고 하던 것을 단순하게

> **"MVC 패턴으로 짰습니다"**

라고 설명하면 패턴을 아는 사람들 모두가 간단하게 이해할 수 있게 되었다. 이 외에도 수많은 패턴들이 있는데, 사람들에게 엿을 먹이려고 이름을 복잡하게 만든 것이 아니고 간단하게 설명하기 위해 만든 것이니 많이 사용되는 디자인 패턴들은 반드시 공부를 하도록 하자!

### MVC는 웹에 한정된 것은 아니다

위에서 설명했듯 디자인 패턴은 설계에 관련된 방법을 정리한 것이다. 그러므로 웹뿐만 아니라 간단한 콘솔 프로젝트에서도 적용할 수 있는데, MVC 패턴을 적용하지 않은 예시와 적용한 예시를 비교하며 MVC 패턴이 왜 필요한지에 대해 알아보자.

#### 요구사항

* 중고거래 장터를 만들어야 한다
* 사용자가 상품명과 금액을 입력할 수 있어야 한다.
* 상품이 정상적으로 등록 될 때 결과를 확인할 수 있어야 한다.

### MVC 패턴 없이 만들기

```java
import java.util.Scanner;

public class Application {
    public static void main(String[] args) {
        class Money {
            private int amount;

            public Money(String amount) {
                int amountParsed;
                try {
                    amountParsed = Integer.parseInt(amount);
                } catch (NumberFormatException e) {
                    throw new IllegalArgumentException("잘못된 형식을 입력하셨습니다.");
                }
                if (amountParsed <= 0) {
                    throw new IllegalArgumentException("잘못된 금액을 입력하셨습니다.");
                }
                this.amount = amountParsed;
            }
        }

        class Item {
            private String name;
            private Money price;

            public Item(String name, Money price) {
                this.name = name;
                this.price = price;
            }

            @Override
            public String toString() {
                return "상품명: " + name + " 금액: " + price;
            }
        }

        Scanner scanner = new Scanner(System.in);
        System.out.println("중고나라에 오신 것을 환영합니다!");
        System.out.println("물품명을 입력해주세요.");
        String name = scanner.nextLine();
        System.out.println("금액을 입력해주세요.");
        Money price = new Money(scanner.nextLine());
        Item item = new Item(name, price);
        System.out.println(item);
    }
}
```

`main` 함수를 실행하면, 우리가 원한 요구사항대로 진행된다. 어렵지 않은 코드이지만, 만일 이 프로그램에서 쓰이는 클래스가 Item이나 Money뿐 아니라 10가지의 클래스가 더 있다면 어떻게 될까? 코드의 유지보수는 갈수록 어려워지고, 에러가 날 확률도 높을 것이다. 어떻게 이 코드를 유지보수가 용이하도록 바꿀 수 있을까?

### MVC 패턴을 적용하여 만들기

#### 코드를 분리해야 한다

특정 기능을 담당하는 코드들로 분리하고 패키지와 import 문을 사용한다면 구조가 지금 상태보단 명확해질 것이다. 그렇다면 이를 어떻게 분리하냐는 문제에 부딪치게 되는데, 이 때 이런 구조를 잡는데 도와주는 것이 디자인 패턴이고, 우리는 MVC 패턴을 적용해 패키지를 나눠볼 것이다. Model을 관리하는 패키지, View를 담당하는 패키지, Controller를 담당하는 패키지를 만들어 코드를 분리해보자. 위에서 언급했듯, Model과 View와 Controller를 이 상황에서 정의하면 다음과 같다.

* **Model (Domain):** 정보를 담고 있으며, 안에 이를 이용하는 메소드들이 있는 클래스들 (`Money`, `Item`)
* **View:** Output과 Input을 담당 (`Scanner`, `System.out`)
* **Controller:** Model과 View를 연결하여 프로그램 전반적인 로직을 수행

![이를 적용하여 프로젝트를 다시 구성하면 다음과 같아진다.](../.gitbook/assets/Screen\_Shot\_2020-02-24\_at\_10.02.06.png)

#### `domain/Item.java`

```java
package domain;

public class Item {
    private String name;
    private Money price;

    public Item(String name, Money price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public String toString() {
        return "상품명: " + name + ", 금액: " + price;
    }
}
```

#### `domain/Money.java`

```java
package domain;

public class Money {
    private int amount;

    public Money(String amount) {
        int amountParsed;
        try {
            amountParsed = Integer.parseInt(amount);
        } catch (NumberFormatException e) {
            throw new IllegalArgumentException("잘못된 형식을 입력하셨습니다.");
        }
        if (amountParsed <= 0) {
            throw new IllegalArgumentException("잘못된 금액을 입력하셨습니다.");
        }
        this.amount = amountParsed;
    }

    @Override
    public String toString() {
        return Integer.toString(amount);
    }
}
```

`domain` 패키지에는 프로그램에서 객체로 사용되는, 어떤 데이터를 가지고 있어야 하는 모델 용도의 클래스들이 모였다.

#### `view/InputView.java`

```java
package view;

import java.util.Scanner;

public class InputView {
    private static Scanner scanner = new Scanner(System.in);

    public static String getInput() {
        return scanner.nextLine();
    }
}
```

#### `view/OutputView.java`

```java
package view;

import domain.Item;

public class OutputView {
    public static void printTitle() {
        System.out.println("중고나라에 오신 것을 환영합니다!");
    }

    public static void nameInstruction() {
        System.out.println("물품명을 입력해주세요.");
    }

    public static void priceInstruction() {
        System.out.println("금액을 입력해주세요.");
    }

    public static void printItem(Item item) {
        System.out.println(item);
    }
}
```

`view` 패키지 내에는 `Scanner`, 또는 `System.out` 등을 이용한 `static` 한 메소드들이 있는데, 각각 input과 output을 담당하며, 메소드명을 통해 그 용도를 명시해주고 있다.

#### `controller/MarketController.java`

```java
package controller;

import domain.Item;
import domain.Money;
import view.InputView;
import view.OutputView;

public class MarketController {
    public static void run() {
        OutputView.printTitle();
        OutputView.nameInstruction();
        String name = InputView.getInput();
        OutputView.priceInstruction();
        Money price = new Money(InputView.getInput());
        Item item = new Item(name, price);
        OutputView.printItem(item);
    }
}
```

`controller` 패키지 내에는 `domain`과 `view`를 연결해주는 용도의 파일이 들어있으며, 지금은 간단한 프로그램을 작성한 것이기 때문에 하나의 `controller` 밖에 없지만, 프로그램이 복잡해져도 기능별로 `controller`들을 더 만들어 분리할 수 있다.

#### `Application.java`

```java
import controller.MarketController;

public class Application {
    public static void main(String[] args) {
        MarketController.run();
    }
}
```

기존에 최상단 패키지에 있던 `Application.java` 는 이제 `MarketController`의 `run` 함수를 실행해주는, 프로그램의 엔트리 포인트(시작/진입점) 역할만을 한다.

### 프로그램의 기능은 동일하다. 하지만 확장성은?

지금 우리는 단순히 코드를 패키지별로 나눴을 뿐이지, 실제 코드에는 아무런 변화가 없으며 프로그램 역시 이전과 동일하게 작동한다. 그렇다면 우리는 왜 이렇게 번거롭게 코드들을 나누고 있는 것일까?

* 데이터를 담는 클래스가 새롭게 필요하다면 `domain`에 추가하면 된다
* 입/출력에 관련된 메소드가 새롭게 필요하다면 `view`에 추가하면 된다.
* `domain`과 `view`에 새롭게 추가된 메소드들을 `controller`에서 다루면 된다.

이렇듯 프로그램에 새로운 기능이 추가되거나 요구사항이 변경될 때 유지보수가 더 용이해진다.

#### MVC가 만능은 아니다

디자인 패턴은 우리보다 먼저 프로그래밍을 하며 설계의 문제점을 찾고 해결했던 선배들이 남겨준 노하우이며, 그렇기에 용도에 맞는, 더 적절한 패턴을 찾아보고 장단점을 확실하게 비교하여 적용할 필요가 있다. 당장 MVC만 보더라도, 여기에서 파생된 MVVM 등의 다른 패턴들이 있으며 어떤 경우에는 MVC 패턴을 적용하는 것이 오히려 더 비효율적일 수도 있다. 프로그램을 만들기에 앞서 요구사항을 분석하고, 경우에 맞는 최적의 패턴을 찾아 적용하거나, 또는 만들어내는 것 또한 프로그래머로서의 중요한 자질이 아닐지 생각해본다.
