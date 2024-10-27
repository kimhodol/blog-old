# Jest: 비동기 & Setup/Teardown

### 비동기로 작성된 코드도 테스트를 해야하는데

JavaScript, TypeScript 환경에서는 데이터베이스에 접근하거나, File IO를 하게 되면 필연적으로 비동기 코드를 작성하게 된다. 그 때 일반적인 검증으로는 아무일도 없이 테스트가 Success로 떠버린다. 콜백함수가 실행되기 전에 테스트가 종료된 것으로 처리되기 때문이다.

```typescript
test('timeout', () => {
  setTimeout(() => {
    expect(1).toBe(2);
  }, 5000);
});
```

```bash
$ npm test

PASS  src/calculator.spec.ts
  ✓ timeout

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        2.4 s
Ran all test suites.
```

분명 1은 2가 아닌데, Fail이 나지 않는다. 그리고 분명 5000ms 이상이 걸려야하는 테스트인데 `timeout` 테스트에 소요된 시간이 없는 것으로 나온다.

그래서 이런 비동기 함수를 테스트하기 위해서 Jest에서는 몇 가지 방법을 제시하는데, `done`을 사용하는 방법과 `async`/`await`를 사용하는 방법 두 개를 알아보자.

#### `done`을 사용해서 비동기 함수를 검증

`test`에 두번째 인자로 들어가는 콜백함수가 `done`이라는 인자를 가지게 되면, `done`이 실행될때까지 테스트가 끝나지 않는다. 다음 예제를 통해 이를 확인하자.

```typescript
test('timeout', (done) => {
  setTimeout(() => {
    expect(1).toBe(2);
    done();
  }, 5000);
});
```

```bash
$ npm test

FAIL  src/calculator.spec.ts (6.793 s)
  ✕ timeout (5022 ms)

  ● timeout

    expect(received).toBe(expected) // Object.is equality

    Expected: 2
    Received: 1

      1 | test('timeout', (done) => {
      2 |   setTimeout(() => {
    > 3 |     expect(1).toBe(2);
        |               ^
      4 |     done();
      5 |   }, 5000);
      6 | });

      at src/calculator.spec.ts:3:15
```

이제 정상적으로 1은 2가 아니라는 fail이 나타난다.

#### `async`/`await`로 사용

`setTimeout`은 Promise를 반환하지는 않으니, 조금 더 Promise를 활용하는 다른 케이스를 살펴보자.

```typescript
test('timeout', () => {
  const one = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(1);
    }, 5000);
  });
  expect(one).toBe(2);
});
```

```bash
$ npm test

FAIL  src/calculator.spec.ts
  ✕ timeout (4 ms)

  ● timeout

    expect(received).toBe(expected) // Object.is equality

    Expected: 2
    Received: {}

       7 |     }, 5000);
       8 |   });
    >  9 |   expect(one).toBe(2);
         |               ^
      10 | });
      11 | 

      at Object.<anonymous> (src/calculator.spec.ts:9:15)
```

테스트가 Fail하긴 했는데... Received가 1이 아닌 `{}`로 나타나고, 소요된 시간도 짧다. `async`/`await`를 이용해서 이를 바꿔보자.

```typescript
test('timeout', async () => {
  const one = await new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(1);
    }, 5000);
  });
  expect(one).toBe(2);
});
```

```bash
$ npm test

FAIL  src/calculator.spec.ts (5.562 s)
  ✕ timeout (5005 ms)

  ● timeout

    expect(received).toBe(expected) // Object.is equality

    Expected: 2
    Received: 1

       7 |     }, 5000);
       8 |   });
    >  9 |   expect(one).toBe(2);
         |               ^
      10 | });
      11 | 

      at src/calculator.spec.ts:9:15
      at step (src/calculator.spec.ts:33:23)
      at Object.next (src/calculator.spec.ts:14:53)
      at fulfilled (src/calculator.spec.ts:5:58)
```

이제 Received도 우리가 생각한 1이 나타난다. 시간도 5000m이 넘게 걸렸다. 비동기 코드를 작성할 때 `done`, `async`/`await`를 이용해야 정상적으로 테스트 가능하다는 것을 알아봤다.

### DRY: Do not Repeat Yourself

테스트를 하게 될 때 데이터를 미리 세팅해 놓는다던지, 테스트가 끝난 후 데이터를 모두 삭제한다던지의 전후작업을 해야할 때가 있다. 이런 전후작업은 크게 2가지로 경우(전/후까지 따지면 4가지) 나뉜다.

1. 각각의 테스트가 실행되기 전/후 매번 실행되야 하는 것 (`beforeEach`, `afterEach`)
2. 모든 테스트가 실행되기 전/후로 한 번 실행되야 하는 것 (`beforeAll`, `afterAll`)

이를 위해서 Jest에서는 Setup과 Teardown 함수를 제공하는데, 각각 테스트 전후에 어떤 작업을 할지 명시할 수 있다. 위의 설명이 와닿지 않는다면 `beforeAll`과 `beforeEach`는 어떤 차이가 있는지 알아보자.

#### `beforeAll`/`afterAll`: 모든 테스트 전후로 한 번 실행

```typescript
const fruits = ['apple', 'banana'];

beforeAll(() => {
  fruits.push('melon');
});

test('fruits의 length가 3이다.', () => {
  expect(fruits).toHaveLength(3);
});

test('fruits의 length가 4다.', () => {
  fruits.push('plum');
  expect(fruits).toHaveLength(4);
});
```

```bash
$ npm test

PASS  src/calculator.spec.ts
  ✓ fruits의 lenght가 3이다. (1 ms)
  ✓ fruits의 length가 4다.

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        1.16 s, estimated 2 s
Ran all test suites.
```

여기서 두 가지 더 알아가게 되는 것이 있다. 1. 테스트는 실행되는 Scope를 공유한다. 2. 테스트는 위에서부터 순차적으로 실행된다.

테스트들이 실행되기 전 `beforeAll`이 한 번 실행되므로 첫번째 테스트에서 `fruits`는 `['apple', 'banana', 'melon']`인 상태가 검증이 되어 총 길이는 3이 된다. 그 다음 테스트에서는 `plum`을 추가했기 때문에 총 길이는 4가 된다. 이제 `beforeAll`을 `beforeEach`로 바꿔보자.

#### `beforeEach`/`afterEach`: 개별 테스트 각각 전후로 실행

```typescript
const fruits = ['apple', 'banana'];

beforeEach(() => {
  fruits.push('melon');
});

test('fruits의 lenght가 3이다.', () => {
  expect(fruits).toHaveLength(3);
});

test('fruits의 length가 4다.', () => {
  fruits.push('plum');
  expect(fruits).toHaveLength(4);
});
```

```bash
$ npm test

FAIL  src/calculator.spec.ts
  ✓ fruits의 lenght가 3이다. (1 ms)
  ✕ fruits의 length가 4다. (1 ms)

  ● fruits의 length가 4다.

    expect(received).toHaveLength(expected)

    Expected length: 4
    Received length: 5
    Received array:  ["apple", "banana", "melon", "melon", "plum"]

      11 | test('fruits의 length가 4다.', () => {
      12 |   fruits.push('plum');
    > 13 |   expect(fruits).toHaveLength(4);
         |                  ^
      14 | });
      15 | 

      at Object.<anonymous> (src/calculator.spec.ts:13:18)
```

`beforeEach`는 `beforeAll`과 달리 모든 테스트별로 실행 전에 한 번씩 되므로 두번째 테스트에 있어서는 `melon`이 `beforeEach`에 의해 두 번 실행된 상태가 되므로, 총 길이가 5가 나오기 때문에 Fail하는 것을 확인 할 수 있다.

이렇게 테스트의 전후작업을 도와주는 Setup과 Teardown 함수를 이용해 중복되는 많은 부분을 줄일 수 있다. 데이터베이스와 관련된 기능을 테스트 할 때 사전 처리, 사후 처리 등을 할 때 많은 도움이 되니 꼭 언제 어떤 함수가 불리는지 이해하고 사용해보자.
