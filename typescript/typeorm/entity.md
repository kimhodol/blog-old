# Entity로 테이블 만들기

#### ORM이 없다면

**Entity**는 데이터베이스 테이블로 변환(mapping)되는 `class`다. ORM이 없을 때 우리가 `user` 테이블을 만들고 다음과 같이 쿼리를 작성한다.

```sql
CREATE TABLE user (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    firstName VARCHAR(255) NOT NULL,
    lastName VARCHAR(255) NOT NULL,
    age INTEGER NOT NULL
);

INSERT INTO user (firstName, lastName, age)
VALUE ('Hodol', 'Kim', 28);

SELECT * FROM user;
```

#### `@Entity()` 살펴보기

Entity는 여기서 `CREATE TABLE user` 부분에 해당하는 것을 만들어주는 역할을 한다. `src/entity/User.ts`를 하나씩 살펴보자.

```typescript
import {Entity, PrimaryGeneratedColumn, Column} from "typeorm";

@Entity()
export class User {

    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    firstName: string;

    @Column()
    lastName: string;

    @Column()
    age: number;
}
```

* `@Entity()` 데코레이터는 `User`가 TypeORM의 Entity로 관리되는 것을 표시한다. 기본적으로 `class`명을 camelCase화해서 테이블명으로 사용하는데, 기존에 존재하는 테이블을 쓰거나 사용할 수 없는 키워드(ex. `Order` 등)을 사용하는 경우 `@Entity("my_user")`처럼 인자를 추가해 테이블 명을 명시할 수 있다.
* `@PrimaryGeneratedColumn()`은 위의 SQL과 비교했을 때 `AUTO_INCREMENT`와 `PRIMARY KEY`를 표시해주는데, 데이터베이스를 어떤 것(Postgres, MySQL 등)을 쓰냐에 따라 `AUTO_INCREMENT`, `SERIAL`, `SEQUENCE` 중에 지원되는 기술을 사용한다. 키 값이 자동으로 생성되지 않는 `@PrimaryColumn()`도 사용할 수 있고, `string` 자료형의 `id`를 사용하는 경우 `"uuid"` 옵션을 넣어 UUID 값을 사용할 수 있다.
* `@Column`은 각각 테이블의 필드가 된다. 기본적으로 필드의 자료형(`string`, `number` 등에 맞는 자료형이 입력되는데`@Column("int")`, `@Column("text")`, `@Column({ type: "text" })`처럼 데코레이터의 인자값에 필드의 속성(`length`, `nullable` 등)을 정의할 수 있다. [여기](https://typeorm.io/#/entities/column-types)서 지원되는 더 많은 정보를 확인할 수 있다.

#### 특수한 `@Column()`

`@CreatedDateColumn()`, `@UpdatedDateColumn()`, `@DeletedDateColumn()`, `@VersionColumn()`을 비롯해 특수한 `@Column()`들을 추가할 수 있다. 생성된 날짜나 수정될 때마다 버전을 바꿔주는 열을 추가해주는데, 이런 특수한 `@Column()`들은 생성시 값을 넣지 않아도 자동으로 생성된다.

```typescript
@CreateDateColumn()
createdAt: Date;

@VersionColumn()
version: number;
```

#### 뭔가 불변객체로 사용하고 싶은데

**모든 필드에 `setter`가 열려있는 것을 보니 뭔가 불-편**했다. 그래서 `readonly` 키워드를 이용해 `setter`를 없애고, `constructor`을 이용해 객체를 생성할 수 있도록 변경해봤다.

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

> [When using an entity constructor its arguments must be optional. Since ORM creates instances of entity classes when loading from the database, therefore it is not aware of your constructor arguments.](https://typeorm.io/#/entities/what-is-entity) `constructor`을 만들 때 인자가 optional 해야한다고 한다는데... 굳이 하지 않아도 작동하는 이유를 잘 모르겠다. 아시는 분이 계신다면 댓글을 부탁드립니다!
