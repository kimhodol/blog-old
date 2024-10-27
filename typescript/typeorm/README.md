# TypeORM

> [TypeORM Getting Started](https://typeorm.io/#/)를 정리한 글입니다.

ORM(Obeject relational mapping)은 어떻게든 SQL문을 최소한으로 작성하며 생산성을 높일지에 대한 고민이 담긴 기술로, 많은 언어에서 라이브러리 또는 프레임워크를 통해 지원한다. Java 진영에서는 Hibernate, Spring Data JPA를, Python에서는 Django ORM과 SQLAlchemy를, Ruby에서는 Active Record를 경험해봤다.

JavaScript, TypeScript 진영에서도 Sequelize, TypeORM 등을 찾을 수 있었다. [NestJS](https://docs.nestjs.com/techniques/database)에서는 기본적으로 RDBMS를 사용할 때 TypeORM을 지원하니 정리해봐야겠다는 생각이 들었다.

### Docker로 [MySQL](https://hub.docker.com/\_/mysql) 환경 설정하기

#### MySQL 컨테이너 띄우기

```bash
docker run --name testdb -e MYSQL_ROOT_PASSWORD=password -d mysql:8.0.23
```

MySQL 8.0.23을 띄워두고, `testdb`라는 별칭으로 접근할 수 있게 해놨다. 혹시 Docker을 몰라도 [설치](https://www.docker.com/products/docker-desktop)하면 위 명령어 하나로 쉽게 컨테이너를 띄울 수 있다.

#### 컨테이너 속 MySQL 클라이언트 접속

```bash
docker exec -ti testdb /bin/bash
mysql -uroot -ppassword
```

#### MySQL에 새로운 데이터베이스 생성

앞으로 우리가 사용할 `photo_app` 데이터베이스를 생성해주자.

```sql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> CREATE DATABASE photo_app;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| photo_app          |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
```

> **Docker 사용법이 궁금하다면 다음 훌륭한 글들을 참고하자.**
>
> * [왜 굳이 도커(컨테이너)를 써야 하나요?](https://www.44bits.io/ko/post/why-should-i-use-docker-container)
> * [도커(Docker) 입문편](https://www.44bits.io/ko/post/easy-deploy-with-docker)
> * [도커(Docker) 컴포즈를 활용하여 완벽한 개발 환경 구성하기](https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose)

### TypeORM 설정하기

```bash
$ npm init -y
$ npm i typeorm reflect-metadata
$ npx typeorm init --database mysql # mysql 드라이버를 사용
```

`typeorm init` 명령어를 통해 폴더 내에 `src/entity`, `src/migration`과 `ormconfig.json`, `tsconfig` 등이 생성된다.

#### `ormconfig.json` 살펴보기

```javascript
{
   "type": "mysql",
   "host": "localhost",
   "port": 3306,
   "username": "test",
   "password": "test",
   "database": "test",
   "synchronize": true,
   "logging": false,
   "entities": [
      "src/entity/**/*.ts"
   ],
   "migrations": [
      "src/migration/**/*.ts"
   ],
   "subscribers": [
      "src/subscriber/**/*.ts"
   ],
   "cli": {
      "entitiesDir": "src/entity",
      "migrationsDir": "src/migration",
      "subscribersDir": "src/subscriber"
   }
}
```

* `type`에는 어떤 드라이버를 사용할지 명시해준다. `init` 시에 `--database mysql`을 썼기에 기본으로 `mysql`이 들어간다.
* `host`, `port`, `username`, `password`, `database`는 데이터베이스의 접속 정보를 가지고 있는데, 우리는 위에서 `root`의 password를 `password`로 설정해줬으니 이에 맞게 설정한다. `database`는 MySQL 안에서 어떤 `database`를 쓸지에 대한 내용인데, `photo_app`으로 설정해두자.
* `synchronize`는 우리가 작성할 Entity(Model)과 데이터베이스 테이블 상태를 동기화 할지에 대한 여부이다. 개발 시에는 `true`로 하되, `production` 환경 등에서 이미 있는 테이블을 사용한다거나 할때 반드시 `false`로 바꿔주자.
* `entities`, `migrations`, `subscribers`는 TypeORM에서 사용할 Entity, Migration, Subscriber 파일을 관리하기 위해 경로 및 파일명 패턴을 정해준다.

> **잠깐 용어 정리**
>
> * **Entity**는 데이터베이스의 테이블 스키마로 변환될 파일이다. 이후에 설명하겠지만, `Photo`, `User`, `PhotoMetadata` 등 하나의 테이블과 매칭될 `.ts` 파일이라고 생각하자.
> * **Migration**은 데이터베이스 테이블 스키마의 변경사항을 명시할 수 있다. `synchronize`가 `true`인 경우 쓰일 일은 잘 없지만, 나중에 테이블 구조를 변경하기 위해 `migration` 명령어를 통해 변경사항을 적용하거나 롤백할 수 있다.
> * **Subscriber**은 TypeORM에서 어떤 이벤트(데이터 삽입, 수정, 삭제 등)가 일어나면 특정 함수를 실행할 수 있도록 하는 파일이다.

#### `package.json` 살펴보기

`init`을 통해 프로젝트를 설저하게 되면 `package.json`에 필수 의존성들을 포함한 기본적인 설정들이 담긴다.

```javascript
{
   "name": "typeorm-sample",
   "version": "1.0.0",
   "description": "",
   "main": "index.js",
   "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "start": "ts-node src/index.ts"
   },
   "keywords": [],
   "author": "",
   "license": "ISC",
   "dependencies": {
      "mysql": "^2.14.1",
      "reflect-metadata": "^0.1.10",
      "typeorm": "0.2.31"
   },
   "devDependencies": {
      "@types/node": "^8.0.29",
      "ts-node": "3.3.0",
      "typescript": "3.3.3333"
   }
}
```

`dependencies`에 `mysql`이 추가되고, `devDependencies`에 `@types/node`, `ts-node`, `typescript`가 포함된 것을 확인할 수 있다. `scripts`의 경우 `start`를 할 때 `ts-node src/index.ts`를 해주는 부분이 추가되어있다. 우선 새로 추가된 의존성들을 설치해주고, 실행해보자.

```bash
$ npm i
$ npm start

Error: ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client
    at Handshake.Sequence._packetToError (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/protocol/sequences/Sequence.js:47:14)
    at Handshake.ErrorPacket (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/protocol/sequences/Handshake.js:123:18)
    at Protocol._parsePacket (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/protocol/Protocol.js:291:23)
    at Parser._parsePacket (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/protocol/Parser.js:433:10)
    at Parser.write (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/protocol/Parser.js:43:10)
    at Protocol.write (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/protocol/Protocol.js:38:16)
    at Socket.<anonymous> (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/Connection.js:88:28)
    at Socket.<anonymous> (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/Connection.js:526:10)
    at Socket.emit (events.js:315:20)
    at Socket.EventEmitter.emit (domain.js:467:12)
    --------------------
    at Protocol._enqueue (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/protocol/Protocol.js:144:48)
    at Protocol.handshake (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/protocol/Protocol.js:51:23)
    at PoolConnection.connect (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/Connection.js:116:18)
    at Pool.getConnection (/Users/hodol/Development/typeorm-sample/node_modules/mysql/lib/Pool.js:48:16)
    at /Users/hodol/Development/typeorm-sample/src/driver/mysql/MysqlDriver.ts:894:18
    at new Promise (<anonymous>)
    at MysqlDriver.createPool (/Users/hodol/Development/typeorm-sample/src/driver/mysql/MysqlDriver.ts:891:16)
    at MysqlDriver.<anonymous> (/Users/hodol/Development/typeorm-sample/src/driver/mysql/MysqlDriver.ts:344:36)
    at step (/Users/hodol/Development/typeorm-sample/node_modules/tslib/tslib.js:141:27)
    at Object.next (/Users/hodol/Development/typeorm-sample/node_modules/tslib/tslib.js:122:57) {
  code: 'ER_NOT_SUPPORTED_AUTH_MODE',
  errno: 1251,
  sqlMessage: 'Client does not support authentication protocol requested by server; consider upgrading MySQL client',
  sqlState: '08004',
  fatal: true
}
```

정상적으로 실행되지 않고 에러가 발생한다. `'Client does not support authentication protocol requested by server; consider upgrading MySQL client'`를 보니 데이터베이스와 드라이버의 호환성 문제로 보인다. 메세지를 검색해보면 MySQL 8.x와 `mysql` 라이브러리를 사용한 커넥션에서 보안 관련 이슈가 있다는 것을 알 수 있다. 이를 간단히 해결하기 위해 `mysql` 의존성을 삭제하고, `mysql2`를 추가해주자.

```bash
$ npm uninstall mysql
$ npm install mysql2
```

이제 다시 실행해보자.

```bash
$ npm start

> typeorm-sample@1.0.0 start
> ts-node src/index.ts

Inserting a new user into the database...
Saved a new user with id: 1
Loading users from the database...
Loaded users:  [ User { id: 1, firstName: 'Timber', lastName: 'Saw', age: 25 } ]
Here you can setup and run express/koa/any other framework.
```

에러 메세지가 없어지고, 정상적으로 실행된 것 처럼 보인다. 뭔가 `User`가 추가되었다는데, 실행된 파일인 `src/index.ts`를 살펴보자.

```typescript
import "reflect-metadata";
import {createConnection} from "typeorm";
import {User} from "./entity/User";

createConnection().then(async connection => {

    console.log("Inserting a new user into the database...");
    const user = new User();
    user.firstName = "Timber";
    user.lastName = "Saw";
    user.age = 25;
    await connection.manager.save(user);
    console.log("Saved a new user with id: " + user.id);

    console.log("Loading users from the database...");
    const users = await connection.manager.find(User);
    console.log("Loaded users: ", users);

    console.log("Here you can setup and run express/koa/any other framework.");

}).catch(error => console.log(error));
```

아직 모든 코드를 다 파악하기는 어렵지만, `createConnection`을 하고 `.then()` 내부의 콜백함수에서 `new User()`로 인스턴스를 만들어 속성을 정해주고 `connection.manager.save(user)`, `await connection.manager.find(User)`을 실행하고 있는 것을 알 수 있다. 뭔가 `User`를 생성하고 저장한 것처럼 보이는데, 다시 MySQL 컨테이너로 돌아가서 이를 확인해보자.

```sql
mysql> USE photo_app;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+---------------------+
| Tables_in_photo_app |
+---------------------+
| user                |
+---------------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM user;
+----+-----------+----------+-----+
| id | firstName | lastName | age |
+----+-----------+----------+-----+
|  1 | Timber    | Saw      |  25 |
+----+-----------+----------+-----+
1 row in set (0.00 sec)
```

`SHOW TABLES`를 통해 TypeORM이 MySQL에 접근해 `CREATE TABLE user...`을 대신 해준 것을 확인할 수 있다. 그리고 `src/index.ts`에서 봤던 인스턴스가 실제 데이터베이스에 잘 저장된 것을 확인할 수 있다.
