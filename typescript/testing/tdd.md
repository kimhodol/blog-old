# 테스트 주도 개발(TDD)

### 테스트 주도 개발: Test Driven Development

#### Wikipedia 에서의 정의

> 테스트 주도 개발(Test-driven development, TDD)은 매우 짧은 개발 사이클을 반복하는 소프트웨어 개발 프로세스 중 하나이다. 개발자는 먼저 요구사항을 검증하는 자동화된 테스트 케이스를 작성한다. 그런 후에, 그 테스트 케이스를 통과하기 위한 최소한의 코드를 생성한다. 마지막으로 작성한 코드를 표준에 맞도록 리팩토링한다. 이 기법을 개발했거나 '재발견' 한 것으로 인정되는 Kent Beck은 2003년에 TDD가 단순한 설계를 장려하고 자신감을 불어넣어준다고 말하였다.

#### 그래서 TDD는 어떻게 하는 걸까

TDD의 핵심은 Test 코드를 먼저 작성한 뒤 이에 대응하는 Production 코드를 작성한다는 것이다. 다음과 같은 과정을 거친다.

1. 주어진 요구사항을 Edge case를 생각하며 구체적으로 정리한다.
2. 정리한 요구사항 기반으로 Test 코드를 작성한다. 당연히 Production 코드가 없으므로 이 케이스는 실패한다.
3. Test를 성공할 수 있는 최소한의 Production 코드를 작성한다.
4. 작성한 Production 코드를 리팩토링한다.
5. 다른 기능이나 함수들에 대해서도 1 \~ 3을 반복한다.

#### 왜 귀찮게 코드를 2배로 작성해야할까

TDD를 도입하면 최소 단위의 기능을 구현하기 위해 코드를 작성하게 되는데, 이 과정에서 코드의 모듈화가 자연스럽게 진행된다. 또한 이 과정을 반복하면 마지막에 완성된 Production 코드는 개발자가 생각했던 대부분의 Test 케이스들을 거치게 된다. 이를 Test Coverage가 높아진다고 표현한다. 그렇다면 이후 요구사항이 변경되거나 유지보수를 진행할 때 Production 코드의 변경이 생겨도 테스트를 해보면 최소한 이전 코드들의 요구사항은 모두 만족시킨다는 **마음의 평화와 자신감을 얻게 된다.**

> **마음의 평화와 자신감**. 개인적으로 느끼는 TDD의 가장 큰 장점이다.

### TDD로 만드는 문자열 덧셈 계산기

> 늘 그렇듯 [스터디 템플릿](https://github.com/kimhodol/typescript-study-template)에서 `npm install`하고 시작하면 된다.

평행우주의 클라이언트가 당신에게 문자열 덧셈 계산기를 만들어달라고 한다.

#### 👨‍🏫 주어진 요구사항

**⚙️ 기능적 요구사항**

* 쉼표(,) 또는 콜론(:)을 구분자로 가지는 문자열을 전달하는 경우 구분자를 기준으로 분리한 각 숫자의 합을 반환 (예: `""` => 0, `"1,2"` => 3, `"1,2,3"` => 6, `"1,2:3"` => 6)
* 앞의 기본 구분자(쉼표, 콜론)외에 커스텀 구분자를 지정할 수 있다. 커스텀 구분자는 문자열 앞부분의 `"//"`와 `"\n"` 사이에 위치하는 문자를 커스텀 구분자로 사용한다. 예를 들어 `"//;\n1;2;3"`과 같이 값을 입력할 경우 커스텀 구분자는 세미콜론(`;`)이며, 결과 값은 6이 반환되어야 한다.
* 문자열 계산기에 숫자 이외의 값 또는 음수를 전달하는 경우 `Error`를 `throw`한다.

**⚙️ 비기능적 요구사항**

* indent(들여쓰기) depth를 2단계에서 1단계로 줄여라.
  * depth의 경우 `if`문을 사용하는 경우 1단계의 depth가 증가한다. `if`문 안에 `while`문을 사용한다면 depth가 2단계가 된다.
* 메소드의 크기가 최대 10라인을 넘지 않도록 구현한다.
  * method가 한 가지 일만 하도록 최대한 작게 만들어라.
* `else`를 사용하지 마라.

### 요구사항 구체적으로 정리하기

요구사항을 정리할 때는 구체적으로 어떤 입력에 어떤 결과가 나오는지를 정리하며 작성한다.

* 빈 문자열 또는 `null`, `undefined` 값의 경우 0을 반환해야 한다.
  * `""` => 0
  * `null` => 0
  * `undefined` => 0
* 숫자 하나를 문자열로 입력할 경우 해당 숫자를 반환한다.
  * `"1"` => 1
* 숫자 두 개 이상을 컴마(,) 구분자로 입력할 경우 숫자의 합을 반환한다.
  * `"1,2"` => 3
  * `"1,2,3"` => 6
* 구분자를 컴마(,) 이외에 콜론(:)을 사용할 수 있다.
  * `"1,2:3"` => 6
* `"//"`와 `"\n"` 문자 사이에 커스텀 구분자를 지정할 수 있다.
  * `"//;\n1;2;3"` => 6
* 숫자 이외의 값 또는 음수를 전달할 경우 예외가 발생해야 한다.
  * `"a,2"` => `Error`
  * `"-1,2,3"` => `Error`

### Test 코드 작성

정리된 첫번째 요구사항에 해당하는 테스트를 작성해보자. `stringCalculator.spec.ts`를 작성한다.

> 이 때, `stringCalculator.ts`는 아직 존재하지 않는다. 테스트를 먼저 작성할 것!

```typescript
// src/stringCalculator.spec.ts
describe('문자열 계산기의 입력이', () => {
  it('빈 문자열의 경우 0을 반환한다.', () => {
    expect(calculate('')).toBe(0);
  });

  it('null의 경우 0을 반환한다.', () => {
    expect(calculate(null)).toBe(0);
  });

  it('undefined의 경우 0을 반환한다.', () => {
    expect(calculate(null)).toBe(0);
  });![](https://images.velog.io/images/kimhodol/post/0d16cbb1-8f90-4e6f-928a-5126866f978f/Screen%20Shot%202021-01-31%20at%2019.50.08.png)
});
```

실제 화면은 다음과 같다. `calculate` 함수가 존재하지 않으므로 당연히 에디터에서는 빨간 줄을 보여줄 것이다.

![](https://images.velog.io/images/kimhodol/post/743e4b73-82d3-4101-88b2-8d5aae1c4d86/Screen%20Shot%202021-01-31%20at%2020.06.50.png)

### 최소한의 Production 코드 작성

이제 테스트에 대응할 수 있게 `stringCalculator.ts`를 작성하고 `calculate` 함수를 만들자. 그리고 `stringCalculator.spec.ts`에서는 이를 `import` 해오자.

```typescript
// src/stringCalculator.ts
export const calculate = (value: string) => {
  return 0;
};
```

실제 화면은 다음과 같다. 필자의 경우 테스트 작성 시 pane을 나눠서 사용하는 방법을 선호한다. ![](https://images.velog.io/images/kimhodol/post/afd93927-ac75-42eb-9872-646617b2db01/Screen%20Shot%202021-01-31%20at%2019.56.32.png)

이대로 테스트를 돌리면 어떻게 될까?

```bash
$ npm test   

FAIL  src/stringCalculator.spec.ts
  ● Test suite failed to run

    src/stringCalculator.spec.ts:9:22 - error TS2345: Argument of type 'null' is not assignable to parameter of type 'string'.

    9     expect(calculate(null)).toBe(0);
                           ~~~~
    src/stringCalculator.spec.ts:13:22 - error TS2345: Argument of type 'undefined' is not assignable to parameter of type 'string'.

    13     expect(calculate(undefined)).toBe(0);
                            ~~~~~~~~~

Test Suites: 1 failed, 1 total
Tests:       0 total
Snapshots:   0 total
Time:        2.303 s
Ran all test suites.
```

실수로 `calculate` 인자로 `string`만 받게 해서 `null`이나 `undefined`는 받지 못하게 했는데, 새로운 `type`을 정의하고 인자의 타입을 바꿔서 이를 처리하고 테스트를 돌려보자.

```typescript
// src/stringCalculator.ts
type Nullable<T> = T | null;

export const calculate = (value?: Nullable<string>) => {
  return 0;
};
```

> [제네릭](https://www.typescriptlang.org/docs/handbook/generics.html)을 사용해서 nullable한 값들을 `Nullable<T>`로 만들고, `calculate`의 인자인 `value`를 `value?`로 바꿔 `undefined`가 들어올 수 있게 했다.

```bash
$ npm test

PASS  src/stringCalculator.spec.ts
  문자열 계산기의 입력이
    ✓ 빈 문자열의 경우 0을 반환한다. (2 ms)
    ✓ null의 경우 0을 반환한다.
    ✓ undefined의 경우 0을 반환한다.

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
Snapshots:   0 total
Time:        2.272 s
Ran all test suites.
```

#### 테스트가 성공하긴 했는데 좀 찜찜하다.

`calculate` 함수는 어떤 입력이 와도 0을 `return`하고 있는데, 테스트 코드에는 0을 `expect` 하는 테스트들밖에 없어 성공해버렸다.

### Production 코드 리팩토링

그럼 이제 테스트 코드에 맞게 Production 코드를 올바르게 리팩토링 해보자.

```typescript
// src/stringCalculator.ts
type Nullable<T> = T | null;

export const calculate = (value?: Nullable<string>) => {
  if (value === null || value === undefined || value.length === 0) {
    return 0;
  }
};
```

이것만 하고 넘어가자. 찜찜하다고? 작성한 요구사항에 기반한 테스트를 작성하기 전까진 다른 코드는 더 작성하지 않는다고 생각하고 해보자.

> 실제로 바쁜 현업에서도 이러라고 하기보단, **입문자 입장에서 테스트를 꼼꼼히 잘 짤 수 있게 해주는 훈련**이 된다고 생각한다. ~~TDD 옹호가분들껜 죄송...~~

### Test 코드 작성 (2)

이제 다음 요구사항인 **"숫자 하나를 문자열로 입력할 경우 해당 숫자를 반환한다."**에 해당하는 테스트를 작성해보자.

```typescript
// src/stringCalculator.spec.ts
import { calculate } from './stringCalculator';

describe('문자열 계산기의 입력이', () => {
  // ...

  it('숫자 하나인 경우 그 숫자를 반환한다.', () => {
    expect(calculate('1')).toBe(1);
  });
});
```

테스트를 돌려보면 다음과 같이 Fail할 것이다. 작성한 조건들 외에는 `undefined`를 `return`하는 함수니까 Fail이 당연한거다.

```bash
$ npm test                                       

FAIL  src/stringCalculator.spec.ts
  문자열 계산기의 입력이
    ✓ 빈 문자열의 경우 0을 반환한다. (2 ms)
    ✓ null의 경우 0을 반환한다.
    ✓ undefined의 경우 0을 반환한다.
    ✕ 숫자 하나인 경우 그 숫자를 반환한다. (3 ms)

  ● 문자열 계산기의 입력이 › 숫자 하나인 경우 그 숫자를 반환한다.

    expect(received).toBe(expected) // Object.is equality

    Expected: 1
    Received: undefined

      15 | 
      16 |   it('숫자 하나인 경우 그 숫자를 반환한다.', () => {
    > 17 |     expect(calculate('1')).toBe(1);
         |                            ^
      18 |   });
      19 | });
      20 | 

      at Object.<anonymous> (src/stringCalculator.spec.ts:17:28)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 3 passed, 4 total
Snapshots:   0 total
Time:        2.436 s
Ran all test suites.
```

### 최소한의 Production 코드 작성 (2)

Fail한 테스트를 성공시키기 위해 최소한의 프로덕션 코드를 또 작성해보자. ~~재밌지 않은가?~~

```typescript
// src/stringCalculator.ts
type Nullable<T> = T | null;

export const calculate = (value?: Nullable<string>) => {
  if (value === null || value === undefined || value.length === 0) {
    return 0;
  }
  return 1;
};
```

```bash
$ npm test

PASS  src/stringCalculator.spec.ts
  문자열 계산기의 입력이
    ✓ 빈 문자열의 경우 0을 반환한다. (2 ms)
    ✓ null의 경우 0을 반환한다.
    ✓ undefined의 경우 0을 반환한다.
    ✓ 숫자 하나인 경우 그 숫자를 반환한다.

Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        2.281 s
Ran all test suites.
```

최소한의 코드로 성공했지만, 이 찝찝한 기분이 들 때 이제 어떡해야하는지 우리는 안다. 리팩토링으로 넘어가보자.

### Production 코드 리팩토링 (2)

`string`을 숫자로 바꿔주기 위해 `parseInt`를 사용한 값을 반환해주자.

```typescript
// src/stringCalculator.ts
type Nullable<T> = T | null;

export const calculate = (value?: Nullable<string>) => {
  if (value === null || value === undefined || value.length === 0) {
    return 0;
  }
  return parseInt(value);
};
```

테스트는 여전히 잘 작동한다.

> 테스트를 매번 돌리는게 귀찮다면, `npm test -- --watch`를 해두면 테스트가 종료되어도 파일에 변경이 생길 때마다 다시 실행된다.

### Test 코드 작성 (3)

이제 다음 요구사항인 **"숫자 두 개 이상을 컴마(,) 구분자로 입력할 경우 숫자의 합을 반환한다."**에 해당하는 테스트를 작성해보자.

```typescript
// src/stringCalculator.spec.ts
import { calculate } from './stringCalculator';

describe('문자열 계산기의 입력이', () => {
  // ...

  it('숫자 두 개 이상을 컴마(,) 구분자로 입력할 경우 숫자의 합을 반환한다.', () => {
    expect(calculate('1,2')).toBe(3);
    expect(calculate('1,2,3')).toBe(6);
  });
});
```

늘 그랬듯 해당 로직이 없으므로 테스트는 실패한다.

```bash
$ npm test           

FAIL  src/stringCalculator.spec.ts
  문자열 계산기의 입력이
    ✓ 빈 문자열의 경우 0을 반환한다. (2 ms)
    ✓ null의 경우 0을 반환한다.
    ✓ undefined의 경우 0을 반환한다.
    ✓ 숫자 하나인 경우 그 숫자를 반환한다.
    ✕ 숫자 두 개 이상을 컴마(,) 구분자로 입력할 경우 숫자의 합을 반환한다. (2 ms)

  ● 문자열 계산기의 입력이 › 숫자 두 개 이상을 컴마(,) 구분자로 입력할 경우 숫자의 합을 반환한다.

    expect(received).toBe(expected) // Object.is equality

    Expected: 3
    Received: 1

      19 | 
      20 |   it('숫자 두 개 이상을 컴마(,) 구분자로 입력할 경우 숫자의 합을 반환한다.', () => {
    > 21 |     expect(calculate('1,2')).toBe(3);
         |                              ^
      22 |     expect(calculate('1,2,3')).toBe(6);
      23 |   });
      24 | });

      at Object.<anonymous> (src/stringCalculator.spec.ts:21:30)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 4 passed, 5 total
Snapshots:   0 total
Time:        1.095 s
Ran all test suites.
```

### 최소한의 Production 코드 작성 (3)

```typescript
// src/stringCalculator.ts
type Nullable<T> = T | null;

export const calculate = (value?: Nullable<string>) => {
  if (value === null || value === undefined || value.length === 0) {
    return 0;
  }
  if (value === '1,2') {
    return 3;
  }
  if (value === '1,2,3') {
    return 6;
  }
  return parseInt(value);
};
```

어처구니가 없겠지만, 필자도 잘 알고 있다. 빠르게 리팩토링 해보자.

> 어처구니 없는 성공 코드가 싫다면, 유도리 있게 리팩토링하며 좋은 코드를 작성해보자. 하지만 이렇게 하는 경우는, **요구사항이 복잡할수록 최소한의 작동하는 코드를 작성하는 것이 오히려 더 쉽기 때문이다.** 지금 우리가 만드는 로직이 단순해서 그렇다고 생각하면 될 것 같다.

### Production 코드 리팩토링 (3)

```typescript
// src/stringCalculator.ts
type Nullable<T> = T | null;

export const calculate = (value?: Nullable<string>) => {
  if (value === null || value === undefined || value.length === 0) {
    return 0;
  }
  if (value?.includes(',')) {
    return value
      .split(',')
      .map((number) => parseInt(number))
      .reduce((previous, current) => previous + current, 0);
  }
  return parseInt(value);
};
```

리팩토링한 코드가 테스트를 잘 통과한다면 이제 무엇을 해야할까? 요구사항에 맞게 다음 테스트를 작성하고, 최소한의 작동하는 프로덕션 코드를 작성하고, 리팩토링한다. 이것을 반복하는 것이 테스트 주도 개발, TDD다.

### TDD Cycle에 따라 나머지 코드도 작성해보자.

![](https://images.velog.io/images/kimhodol/post/78edd33e-326c-434f-89da-0dd011b31836/1\_IbHgZrKYCUSeIbL\_PywObQ.png)

이렇게 우리는 3번의 TDD Cycle을 통해 코드를 작성해보았다. 나머지 요구사항은 직접 구현해본다면 훌륭한 훈련이 될 것이다.

#### TDD를 통해 우리가 얻은 것

* 새로운 기능들이 추가되더라도 이전 기능들이 망가지지 않는지 확인할 수 있고
  * 이건 당장 오늘 작성한 코드 외에 내일 또는 몇 개월 뒤 기능이 추가되어도 마찬가지다.
* 우리가 예측한 범위 내에서 프로덕션 코드는 작동이 보장된다는 확신이 생긴다.
* 짧은 주기의 리팩토링을 통해 기능이 추가됨에도 꾸준히 코드의 품질을 개선할 수 있다.

[개발자 채용에서도 TDD를 심심찮게 볼 수 있는 것](https://resume.woowahan.com/jobs/view.aspx?cc=244001\&jidx=105954\&pidx=11474\&bidx=166014)으로 보아 현업에서도 실제로 쓰이는 곳도 있다는 것을 알 수 있다. TDD는 우아한테크코스 과정에서 처음 학습하게 되었는데 **개발자로서 내가 짠 코드에 책임감을 불어넣어주는 테스트를 잘 작성하는 훈련**이라는 느낌이 들었다. TDD를 통해 많은 테스트를 작성하게 되면 자연스럽게 나중에 빈 테스트를 채워넣을 때도 어떻게 접근해야 할지 감이 온다.

개인적으로는 세포 증식하듯 늘어나는 요구사항과 싸우는 개발자의 특성상, 테스트 코드 작성을 게을리하게 된다. 그래도 작성한 프로덕션 코드가 버그가 날지 기도하며 살아가는 것보단 테스트 작성 주간을 가져서라도 빈 테스트를 채워나가려고 노력하는게 낫다는 생각이 든다.

켄트 벡의 명작 중 하나인 [이 책](https://book.naver.com/bookdb/book\_detail.nhn?bid=7443642)을 보며 테스트 주도 개발에 대해 더 많은 것을 익혀보는 것도 괜찮을 것 같다.
