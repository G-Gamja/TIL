# TypeORM이란?

TypeOrmModule은 TypeORM 패키지를 NestJS 프레임워크와 통합하는 데 사용되는 모듈입니다. 이 모듈을 사용하면 데이터베이스와의 상호작용을 위한 다양한 기능을 NestJS 애플리케이션에서 사용할 수 있습니다.

ORM은 Object-Relational Mapping의 약자로, 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형 시스템 간에 데이터를 변환하는 프로그래밍 기법입니다. 즉, 데이터베이스와 객체 지향 프로그래밍 언어 간의 '다리' 역할을 합니다.

TypeORM은 TypeScript와 JavaScript(ES7, ES6, ES5)를 위한 ORM입니다. 이는 NodeJS, 브라우저, Cordova, PhoneGap, Ionic, React Native, NativeScript 등에서 동작하며, MySQL, PostgreSQL, MariaDB, SQLite, MS SQL Server, Oracle, WebSQL 데이터베이스를 지원합니다. 이를 통해 개발자는 SQL 쿼리를 직접 작성하지 않고도 데이터베이스 작업을 수행할 수 있습니다.

# Repository

TypeORM의 `Repository`는 특정 엔티티에 대한 데이터베이스 연산을 수행하는 메서드를 제공하는 클래스입니다. Repository를 사용하면 데이터베이스에 대한 CRUD(Create, Read, Update, Delete) 연산을 수행할 수 있습니다.

예를 들어, Repository는 다음과 같은 메서드를 제공합니다:

- find: 조건에 맞는 모든 엔티티를 조회합니다.
- findOne: 조건에 맞는 하나의 엔티티를 조회합니다.
- save: 주어진 엔티티를 저장합니다. 엔티티가 이미 데이터베이스에 존재하면 업데이트하고, 그렇지 않으면 새로 생성합니다.
- remove: 주어진 엔티티를 삭제합니다.
- create: 주어진 플레인 자바스크립트 객체를 엔티티 인스턴스로 변환합니다.
  이러한 메서드들을 사용하면 SQL 쿼리를 직접 작성하지 않고도 데이터베이스와 상호작용할 수 있습니다. 이는 코드의 가독성을 높이고, SQL 쿼리에 대한 실수를 줄이는 데 도움이 됩니다.
