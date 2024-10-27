# 더 객체지향적으로

`@Entity()`를 통해 테이블을 만들다보면 아무래도 원시값 위주로 필드를 작성하게 된다. 객체지향을 실컷 배워놓고 이렇게 하는 것은 의미가 없는 것 같다. [Embedded Entities](https://typeorm.io/#/embedded-entities)를 활용해 조금 더 객체지향적인(?) Entity를 만들어보자.

```typescript
import {
  Entity,
  PrimaryGeneratedColumn,
  Column,
  CreateDateColumn,
  VersionColumn,
} from "typeorm";

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  readonly id: number;

  @Column()
  readonly firstName: string;

  @Column()
  readonly lastName: string;

  @Column()
  readonly age: number;

  @CreateDateColumn()
  readonly createdAt: Date;

  @VersionColumn()
  readonly version: number;

  constructor(firstName: string, lastName: string, age: number) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }
}
```

> **왜 `private`을 사용하지 않지?** `connection.manager.save()`를 호출할 때는 괜찮지만, `connection.manager.update()`의 경우 인자로 `QueryDeepPartialEntity<T>`의 타입을 인자로 받게 되는데, 필드가 `private`한 경우 변경사항을 객체 형태로 입력할 때 타입 매칭이 되지 않는다. ~~삽질을 열심히 해봤지만 아직까지는 답을 못 찾은 상태...~~

기존의 `User`에는 자동으로 생성되는 값 외에 `firstName`, `lastName`, `age` 등의 필드가 있다. `firstName`과 `lastName`은 `Name`으로, `age`는 `Age`라는 값 객체로 포장할 수 있을 것 같다. `Name`이나 `Age`는 지금의 `user` 테이블에 함께 저장되기 때문에 독립적인 Entity로 취급되지 않는다.

```typescript
export class Name {
  @Column()
  readonly firstName: string;

  @Column()
  readonly lastName: string;

  constructor(firstName: string, lastName: string) {
    if (firstName.length === 0 || lastName.length === 0) {
      throw new Error("성이나 이름이 빈 값일 수 없습니다.");
    }
    this.firstName = firstName;
    this.lastName = lastName;
  }
}

export class Age {
  @Column()
  readonly age: number;

  constructor(age: number) {
    if (age < 0) {
      throw new Error("나이는 음수일 수 없습니다.");
    }
    this.age = age;
  }
}
```

> **경고!!** 위 코드는 사용시 `constructor` 단의 검증 로직에서 에러를 뿜기때문에... 사용할 수 없다.

`Name`과 `Age`의 값을 각각 포장해줬다. 설명했듯 각각이 `name`, `age` 등의 테이블로 변환되는 것이 아니기 때문에 `@Entity()` 데코레이터가 없다는 것을 확인할 수 있다. 그렇다면 이를 어떻게 `User`에서 사용할까?

```typescript
@Entity()
export class User {
  @PrimaryGeneratedColumn()
  readonly id: number;

  @Column((type) => Name)
  readonly name: Name;

  @Column((type) => Age)
  readonly age: Age;

  @CreateDateColumn()
  readonly createdAt: Date;

  @VersionColumn()
  readonly version: number;

  constructor(name: Name, age: Age) {
    this.name = name;
    this.age = age;
  }
}
```

기존에 `@Column()`이었던 `name`과 `age`에 각각 `(type) => Name`, `(type) => Age`인자가 생겼다. 이렇게 TypeORM에서 Entity를 객체지향스럽게 분리해서 사용할 수 있다.

### 번외: 검증로직에서 에러를 뿜는다

`SELECT`를 해서 데이터를 가져올 떄 위의 `Name`, `Age`의 `constructor` 단에서 에러를 뿜는다. 지난 글에서 **생성자를 모두 Optional하게 해야한다는데 그냥 되네요?**하고 궁금했던 것이 결국 발목을 잡았는데..

> When using an entity constructor its arguments must be optional. **Since ORM creates instances of entity classes when loading from the database, therefore it is not aware of your constructor arguments.**

TypeORM이 데이터베이스에 접근해서 값을 가져올 때, 모든 값을`undefined`로 가져와서 인스턴스를 만들기 때문이다. 즉, `constructor`은 우리가 초기에 생성할 때 인자를 넣어주는 용도정도로만 사용할 수 있고, 내부에 검증로직은 있어서는 안된다. 그렇다면 값 검증을 어떻게 하냐? `class-validator` 라이브러리를 사용해야한다.

```bash
$ npm i class-validator
```

```typescript
export class Name {
  @Column()
  @IsNotEmpty()
  readonly firstName: string;

  @Column()
  @IsNotEmpty()
  readonly lastName: string;

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}

export class Age {
  @Column()
  @IsPositive()
  age: number;

  constructor(age: number) {
    this.age = age;
  }
}

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  readonly id: number;

  @Column((type) => Name)
  @ValidateNested()
  readonly name: Name;

  @Column((type) => Age)
  @ValidateNested()
  readonly age: Age;

  @CreateDateColumn()
  readonly createdAt: Date;

  @VersionColumn()
  readonly version: number;

  constructor(name: Name, age: Age) {
    this.name = name;
    this.age = age;
  }

  @BeforeInsert()
  @BeforeUpdate()
  async validate() {
    await validateOrReject(this);
  }
}
```

조금 코드가 많아졌는데, 하나씩 살펴보자.

1. `@IsNotEmpty()`, `@IsPositive()` 등은 `class-validator`에서 제공되는 것인데, 이 외에도 이메일, 숫자 범위, 커스텀한 정규표현식 등을 정말 다양하게 많은 데이터를 검증할 수 있다. [여기](https://github.com/typestack/class-validator)에서 자세히 확인해보자.
2. `@ValidatedNested()`는 클래스에서 검증하려는 자료형이 원시값이 아닌 경우 사용한다. 우리는 `Name`과 `Age`의 값을 포장했으므로 각각에 검증 데코레이터를 붙여주고, `User`에서는 "이 클래스 내부 검증로직도 한 번 보세요"하고 표현해주기 위해 `@ValidatedNested()`를 사용했다.
3. `@BeforeInsert()`와 `@BeforeUpdate()`는 TypeORM에서 제공하는 [Entity Listener](https://typeorm.io/#/listeners-and-subscribers/what-is-an-entity-listener)로, Entity에 어떤 이벤트가 일어나기 전후로 Hook 같은 로직을 넣을 수 있다. 위 코드에서는 `class-validator`의 함수인 `validateOrReject()`가 `INSERT`, `UPDATE` 전 각각 호출되도록 했다.

> `class-validator`의 `validate()`, `validateOrReject()` 등을 호출해야만 실제로 검증이 이뤄지고, 검증을 통과하지 못할 때 `ValidationError`를 돌려준다.
