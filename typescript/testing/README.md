# Testing

### 오늘의 할 일: 사칙연산 계산기 만들기

#### ⚙️ 기능적 요구사항

* 사칙연산이 가능한 계산기를 만들어보자.
* 작성한 테스트를 `*.spec.ts`를 활용해 테스트를 작성해보자.

### 계산기를 만들어보자.

```typescript
// src/calculator.ts
export const calculate = (
  number1: number,
  number2: number,
  operator: string
) => {
  if (operator === '+') {
    return number1 + number2;
  }
  if (operator === '-') {
    return number1 - number2;
  }

  if (operator === '*') {
    return number1 * number2;
  }

  if (operator === '/') {
    return number1 / number2;
  }
};
```

### 테스트 없이(?) 테스트짜기

작성한 코드가 어떤 결과를 내는지 궁금할 때 어떻게 할까? `console.log`를 이용해보자.

```typescript
// src/index.ts
import { calculate } from './calculator';

console.log(calculate(1, 2, '+')); // 3이 나와야 한다.
console.log(calculate(1, 2, '-')); // -1이 나와야 한다.
console.log(calculate(1, 2, '*')); // 2가 나와야 한다.
console.log(calculate(1, 2, '/')); // 0.5가 나와야 한다.
```

```bash
$ npm start               

3
-1
2
0.5
```

훌륭한 계산기를 만들었다. 그런데, `console.log`를 지우고 나중에 변경사항이 생기면 또 만들어야하지 않을까?

### `console.log` 대신 테스트를 작성해보자

같은 폴더 내에 `calculator.ts` 관련 테스트가 작성될 `calculator.spec.ts`를 만들어보자.

```typescript
// src/calculator.spec.ts
import { calculate } from './calculator';

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

```bash
$ npm test 

PASS  src/calculator.spec.ts
  계산기가
    ✓ 덧셈을 한다. (2 ms)
    ✓ 뺄셈을 한다.
    ✓ 곱셈을 한다.
    ✓ 나눗셈을 한다. (1 ms)

Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        2.36 s
Ran all test suites.
```

이제 `index.ts`에 있는 `console.log`들은 삭제해도 된다.

### 두 방법을 비교해보면

#### `index.ts`에서 `console.log`를 이용해 테스트를 하게 된다면

1. 예상한 결과값이 나오는지 직접 확인해야하고
2. 여러 기능을 테스트하게 되면 복잡도가 증가하고
3. Production code의 크기가 커지고
4. Test code가 실 서비스에 같이 배포된다는 문제점이 있다.

#### 테스트 코드를 분리하면

1. 예상한 결과값이 나오는지 알아서 확인해주고
2. 여러 기능을 여러 테스트 파일에 작성하면 복잡도가 낮아지고
3. 테스트 파일이 분리되기 때문에 Production code의 사이즈가 커지지 않고
4. Test code도 숨길 수 있다.
