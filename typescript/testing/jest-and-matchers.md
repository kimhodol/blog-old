# Jest: 기본기 & Matchers

### 그래서 Jest가 뭔데?

> Jest is a delightful JavaScript Testing Framework with a focus on simplicity. It works with projects using: Babel, TypeScript, Node, React, Angular, Vue and more!

JavaScript/TypeScript 테스트를 위한 프레임워크는 대표적으로 다음과 같다.&#x20;

1. Mocha
2. Jest&#x20;
3. Jasmine
4. Karma
5. Puppeteer

![](https://images.velog.io/images/kimhodol/post/ae249a42-7c9d-4ec3-a072-b8007a694abe/Screen%20Shot%202021-01-30%20at%2013.29.15.png)

Jest의 경우 Facebook에서 적은 설정으로 빠르고 정확하게 JavaScript 테스트를 작성하자는 철학으로 만든 프레임워크인데, [State of JS](https://2020.stateofjs.com/en-US/technologies/testing/)에서 볼 수 있듯 인지도가 제일 높다고 볼 수 있다. 향후에 공부할 Nest.js에서도 기본적으로 Jest를 사용하기 때문에 이를 위주로 살펴볼 예정이다.

### 기본 사용법

```typescript
import { sum } from './sum';

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

`test`, `expect`, `toBe` 함수들이 눈에 띈다. 굉장히 직관적이지만 한 번 설명하자면, 1. `test`는 첫번째 인자로 이 테스트가 어떤 것을 설명하는지 제목을 가진다. 2. 두번째 인자로는 콜백함수를 가지는데, 이 안에서 테스트를 진행한다. 3. 콜백함수 안에 있는 `expect`는 `return`값을 가지는 어떤 함수를 실행한다. 4. `expect`된 객체를 `toBe`를 사용해서 어떤 값이 나올지 인자로 담는다.

> `test` 대신 `it`을 사용할 수 있다. [완전히 같은 alias일 뿐이다.](https://jestjs.io/docs/en/api.html#testname-fn-timeout)

### Matchers로 다양한 결과 예측하기

#### `.toBe`

`toBe` 외에도 많은 Matcher들이 존재하는데, `toBe`의 경우 `Object.is`를 내부적으로 사용해서 비교하는 두 객체를 비교했을 때 `===`이 성립하는지로 이해하면 될 것 같다.

> **엄밀히 말하면 다르긴 한데**, MDN에서 가져온 다음 글을 참고하자.
>
> Object.is() method is not the same as being equal according to the === operator. The === operator (and the == operator as well) treats the number values -0 and +0 as equal and treats Number.NaN as not equal to NaN.

#### `.toEqual`

```typescript
test('object assignment', () => {
  const data = {one: 1};
  data['two'] = 2;
  expect(data).toEqual({one: 1, two: 2});
});
```

`toEqual`은 배열 또는 객체의 값(**주소 말고!**)이 같은지 확인한다.

#### `.toContain`

```typescript
const shoppingList: string[] = [
  'diapers',
  'kleenex',
  'trash bags',
  'paper towels',
  'milk',
];

test('the shopping list has milk on it', () => {
  expect(shoppingList).toContain('milk');
  expect(new Set(shoppingList)).toContain('milk');
});
```

배열이나 iterable의 경우에는 `toContain`으로 값을 가지고 있는지 검증할 수 있다.

#### `.toBeNull`, `.toBeUndefined`, `.toBeDefined`

```typescript
test('null', () => {
  const n = null;
  expect(n).toBeNull();
  expect(n).toBeDefined();
  expect(n).not.toBeUndefined();
});

test('zero', () => {
  const z = 0;
  expect(z).not.toBeNull();
  expect(z).toBeDefined();
  expect(z).not.toBeUndefined();
});
```

`null`, `undefined` 등인지 아닌지를 검증할 수 있다. 또한 matchers 앞에 `.not`을 붙여서 반대 검증을 하는 방법이 있다.

#### 숫자형 관련

```typescript
test('two plus two', () => {
  const value = 2 + 2;
  expect(value).toBeGreaterThan(3);
  expect(value).toBeGreaterThanOrEqual(3.5);
  expect(value).toBeLessThan(5);
  expect(value).toBeLessThanOrEqual(4.5);

  // toBe and toEqual are equivalent for numbers
  expect(value).toBe(4);
  expect(value).toEqual(4);
});

test('adding floating point numbers', () => {
  const value = 0.1 + 0.2;
  //expect(value).toBe(0.3);           This won't work because of rounding error
  expect(value).toBeCloseTo(0.3); // This works.
});
```

숫자형을 비교하는 `toBeGreaterThan`, `toBeGreaterThanOrEqual` 등도 있는데, [부동소수점 문제](https://www.w3schools.com/js/js\_numbers.asp) 때문에 실수형의 경우에는 `toBeCloseTo` 등을 활용해야할 수도 있다는 것을 알고 가자.

#### 예외 발생

```typescript
function compileAndroidCode() {
  throw new Error('you are using the wrong JDK');
}

test('compiling android goes as expected', () => {
  expect(() => compileAndroidCode()).toThrow();
  expect(() => compileAndroidCode()).toThrow(Error);

  // You can also use the exact error message or a regexp
  expect(() => compileAndroidCode()).toThrow('you are using the wrong JDK');
  expect(() => compileAndroidCode()).toThrow(/JDK/);
});
```

개인적으로 예외처리를 위한 테스트를 많이 짜는 편인데, 이렇게 어떤 값이 들어갔을 때 우리가 예측한 예외가 나온다는 것을 검증할 수 있다. 꼭 알아두자. `toThrow`는 자주 쓰이니 꼭 알아두자.
