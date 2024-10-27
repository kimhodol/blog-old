# Jest: Describe/It 패턴

Jest를 다룬 이전 두 글에서 볼 수 없던 것이 있다.

```typescript
describe('계산기가', () => {
  it('덧셈을 한다.', () => {
    expect(calculate(1, 2, '+')).toBe(3);
  });

  it('뺄셈을 한다.', () => {
    expect(calculate(1, 2, '-')).toBe(-1);
  });

  it('곱셈을 한다.', () => {
    expect(calculate(1, 2, '*')).toBe(2);
  });

  it('나눗셈을 한다.', () => {
    expect(calculate(1, 2, '/')).toBe(0.5);
  });
});
```

`it`은 `test`의 alias라 했고, `describe`는 무엇일까?

### `describe`-`it` 패턴으로 가독성 높이기

쉽게 말하면, `test`(또는 `it`)들을 `describe`라는 블록으로 묶음으로서 스코프를 공유하고 가독성을 높이는 것이다. 비즈니스 로직과 관련된 테스트는 기획자를 비롯한 비개발자도 보고 바로 이해할 수 있어야 한다. 늘 그렇듯 패턴을 적용하지 않고 해보는 것이 우선이다.

```typescript
// src/myNumber.ts
export class MyNumber {
  constructor(private value: number) {}

  add(other: number) {
    return new MyNumber(this.value + other);
  }

  subtract(other: number) {
    return new MyNumber(this.value - other);
  }

  times(other: number) {
    return new MyNumber(this.value * other);
  }

  divide(other: number) {
    return new MyNumber(this.value / other);
  }

  equals(other: number) {
    return this.value === other;
  }
}
```

```typescript
// src/myNumber.spec.ts
import { MyNumber } from './myNumber';

let number = new MyNumber(1);
const number2 = new MyNumber(2);

test('1에 2를 더한다.', () => {
  number = number.add(2);
});

test('그리고 3을 곱한다.', () => {
  number = number.times(3);
});

test('그리고 2로 나눈다.', () => {
  number = number.divide(2);
  expect(number.equals(4.5)).toBeTruthy();
});

test('2에서 5를 뺀다', () => {
  expect(number2.subtract(5).equals(-3)).toBeTruthy();
});
```

```bash
$ npm test

PASS  src/myNumber.spec.ts
  ✓ 1에 2를 더한다. (1 ms)
  ✓ 그리고 3을 곱한다.
  ✓ 그리고 2로 나눈다. (1 ms)
  ✓ 2에서 5를 뺀다 (1 ms)

Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        2.619 s
Ran all test suites.
```

~~막상 쓰고 보니 그렇게 좋은 예제 같진 않지만,~~ 우선 테스트 코드에서 무엇을 하고 있는지 살펴보자. 전역에 두 개의 변수를 초기화해주고 이를 여러 테스트에서 사용하고 있다. 세 개의 테스트에서는 `number` 변수를 사용하고 있고, 마지막 테스트는 `number2` 변수만을 사용하고 있다. 이런 경우 같은 스코프를 공유하고 있기 때문에 실수로 다른 변수를 사용할 수도 있다.

처음 두 테스트에서는 `expect`를 이용한 검증 없이 단순히 `number` 변수에 다른 값을 할당하고 있는데, 이를 전역 스코프에서 하자니 찝찝한 것은 사실이다. 테스트를 실행했을 때 나오는 콘솔 창도 같은 문맥을 사용하고 있으니 테스트간의 연관관계가 있다는 착각을 하게 된다.

#### `describe`를 이용해 테스트 코드를 리팩토링해보자.

```typescript
import { MyNumber } from './calculator';

describe('MyNumber을 1로 생성하고', () => {
  let number = new MyNumber(1);
  describe('2를 더하고 3을 곱한다.', () => {
    number = number.add(2);
    number = number.times(3);
    it('2로 나누면 4.5가 나올 것이다.', () => {
      number = number.divide(2);
      expect(number.equals(4.5)).toBeTruthy();
    });
  });
});

describe('MyNumber을 2로 생성하고', () => {
  const number2 = new MyNumber(2);
  it('2에서 5를 빼면 -3이 나올 것이다.', () => {
    expect(number2.subtract(5).equals(-3)).toBeTruthy();
  });
});
```

```bash
$ npm test

PASS  src/myNumber.spec.ts
  MyNumber을 1로 생성하고
    2를 더하고 3을 곱한다.
      ✓ 2로 나누면 4.5가 나올 것이다. (3 ms)
  MyNumber을 2로 생성하고
    ✓ 2에서 5를 빼면 -3이 나올 것이다. (1 ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        2.854 s, estimated 3 s
Ran all test suites.
```

`describe`로 테스트 블록을 분리하여 두 가지 효과를 얻었다. 1. 테스트의 가독성이 높아져 각각 공유되는 스코프와 변수가 한 눈에 보인다. 2. 테스트 실행 시 콘솔 창에 메세지가 nesting 됨으로써 결과를 읽기 쉬워졌다.

그리고 `describe`는 nesting이 가능하다. 각각의 테스트에서 어떤 값을 공유하고 싶은지 고민 후 이를 공유하지 않는 테스트들을 서로 격리시켜주고, 이를 통해 가독성을 높일 수 있다는 것을 확인했다.
