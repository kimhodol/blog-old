# Jest: 학습 테스트 with lodash

### 테스트는 학습의 도구로도 사용할 수 있다.

> 프레임워크나 다른 라이브러리 등에 대한 테스트를 학습테스트(learning test) 라고 한다.

#### 학습 테스트의 장점

출처: [학습 테스트로 배우는 스프링](https://springwiki.readthedocs.io/en/latest/%ED%95%99%EC%8A%B5%20%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%A1%9C%20%EB%B0%B0%EC%9A%B0%EB%8A%94%20%EC%8A%A4%ED%94%84%EB%A7%81/)

* 다양한 조건에 따른 기능을 손쉽게 확인해 볼 수 있다.
* 학습 테스트 코드를 개발 중에 참고할 수 있다.
* 프레임워크나 제품을 업그레이드할 때 호환성 검증을 보여준다.
* 테스트 작성에 대한 좋은 훈련이 된다.
* 새로운 기술을 공부하는 과정이 즐거워진다.

### `lodash`를 한 번도 안 써봤다

나름 JavaScript를 오래 썼다고 생각했는데, 유명한 유틸리티 라이브러리인 [lodash](https://lodash.com/)를 한 번도 사용해보지 않았다. 그런데 문서를 보니 너무 기능이 많다. Jest로 학습 테스트를 진행하며 익혀보자.

> 늘 그렇듯 [스터디 템플릿](https://github.com/kimhodol/typescript-study-template)에서 시작한다.

```bash
$ npm i lodash @types/lodash
```

#### `_cloneDeep` 학습 테스트

> 오늘의 꿀팁: JavaScript/TypeScript에서 [전개연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread\_syntax)를 써도 nested된 객체는 복사가 되지 않는다.

shallow copy말고 deep copy를 해야할 때를 위해 lodash에서 [`cloneDeep`](https://lodash.com/docs/4.17.15#cloneDeep) 함수를 제공한다고 한다. 이를 위해 학습 테스트를 작성해보자.

```typescript
// src/lodash.spec.ts
interface Profile {
  name: string;
  age: number;
  teammates: Profile[];
}

describe('프로필을 만들고', () => {
  let hodol: Profile;
  beforeEach(() => {
    hodol = {
      name: 'woonjang',
      age: 28,
      teammates: [
        {
          name: 'kyle',
          age: 28,
          teammates: [],
        },
      ],
    };
  });
}
```

우선 `Profile` 인터페이스와 이를 구현한 원본 `hodol` 객체를 만든다. 테스트가 수행될 때 매번 `hodol`를 초기화 할 것이므로 `beforeEach`를 사용해 초기화한다.

**전개연산자를 이용한 Shallow Copy를 테스트하기 위해 다음을 테스트한다.**

1. 전개연산자로 객체를 복사했을 때 Primitive type인 필드(이 경우엔 `name`)를 바꾸면
   * 복사본(`hodolClone`)만 수정되고
   * 원본(`hodol`)은 바뀌지 않는다.
2. 하지만 Nested된 배열의 객체 내용(이 경우엔 `teammates[0].name`을 바꾸면
   * 복사본과 원본 모두 수정된다.

```typescript
// ...
describe('프로필을 만들고', () => {
  // ...
  describe('전개연산자로 복사하면', () => {
    let hodolClone: Profile;
    beforeEach(() => {
      hodolClone = { ...hodol };
    });

    it('name을 바꿔도 원본은 변하지 않는다.', () => {
      hodolClone.name = 'roonjang';
      expect(hodolClone.name).toBe('roonjang');
      expect(hodol.name).toBe('woonjang');
    });

    it('teammates의 name을 바꾸면 원본도 변한다.', () => {
      hodolClone.teammates[0].name = 'siyoung';
      expect(hodolClone.teammates[0].name).toBe('siyoung');
      expect(hodol.teammates[0].name).toBe('siyoung');
    });
  });
}
```

```bash
$ npm test

PASS  src/lodash.spec.ts
  프로필을 만들고
    전개연산자로 복사하면
      ✓ name을 바꿔도 원본은 변하지 않는다. (2 ms)
      ✓ teammates의 name을 바꾸면 원본도 변한다.

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        2.535 s
Ran all test suites.
```

생각했던대로 테스트가 수행되었다.

**이제 \_.cloneDeep을 이용해서 Deep Copy를 해보자.**

1. `_.cloneDeep`으로 객체를 복사했을 때 Primitive type인 필드(이 경우엔 `name`)를 바꾸면
   * 복사본(`hodolDeepClone`)만 수정되고
   * 원본(`hodol`)은 바뀌지 않는다.
2. Nested된 배열의 객체 내용(이 경우엔 `teammates[0].name`을 바꾸면
   * 복사본(`hodolDeepClone`)만 수정되고
   * 원본(`hodol`)은 바뀌지 않는다.

```typescript
import _ from 'lodash';
// ...
describe('프로필을 만들고', () => {
  // ...
  describe('전개연산자로 복사하면', () => {
    // ...
  });
  describe('_.cloneDeep으로 복사하면', () => {
    let hodolDeepClone: Profile;
    beforeEach(() => {
      hodolDeepClone = _.cloneDeep(hodol);
    });
    it('name을 바꿔도 원본은 변하지 않는다.', () => {
      hodolDeepClone.name = 'poonjang';
      expect(hodolDeepClone.name).toBe('poonjang');
      expect(hodol.name).toBe('woonjang');
    });

    it('teammates의 name을 바꿔도 원본은 변하지 않는다.', () => {
      hodolDeepClone.teammates[0].name = 'siyoung';
      expect(hodolDeepClone.teammates[0].name).toBe('siyoung');
      expect(hodol.teammates[0].name).toBe('kyle');
    });
  });
});
```

```bash
$ npm test

PASS  src/lodash.spec.ts
  프로필을 만들고
    전개연산자로 복사하면
      ✓ name을 바꿔도 원본은 변하지 않는다. (2 ms)
      ✓ teammates의 name을 바꾸면 원본도 변한다. (1 ms)
    _.cloneDeep으로 복사하면
      ✓ name을 바꿔도 원본은 변하지 않는다. (1 ms)
      ✓ teammates의 name을 바꿔도 원본은 변하지 않는다. (1 ms)

Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        1.253 s
Ran all test suites.
```

`_.cloneDeep`이 문서에 있는대로 작동한다는 것을 확인했다. 이렇게 새로운 프레임워크나 라이브러리를 학습할 때 무작정 `console.log`를 해보며 확인하기보단 학습테스트를 작성해둔다면 나중에 기술을 도입할 때도 참고할 수 있다. 또한 이렇게 문서를 보며 이해하고 예상한대로 작동하는지 확인할 수 있다는 것이 학습테스트의 또 다른 장점이라 할 수 있다.
