1. **DTO (Data Transfer Object)**: DTO는 계층간 데이터 교환을 위한 객체입니다. NestJS에서는 DTO를 사용하여 클라이언트로부터 받은 데이터의 유효성을 검사합니다. 클래스를 사용하여 DTO를 정의하고, 데코레이터를 사용하여 유효성 검사 규칙을 지정할 수 있습니다.

```typescript
import { IsString, IsInt } from "class-validator";

export class CreateCatDto {
  @IsString()
  name: string;

  @IsInt()
  age: number;

  @IsString()
  breed: string;
}
```

2. **DAO (Data Access Object)**: DAO는 데이터베이스의 CRUD(Create, Read, Update, Delete) 연산을 캡슐화하는 객체입니다. NestJS에서는 TypeORM과 같은 ORM 라이브러리를 사용하여 DAO를 구현할 수 있습니다. `GetTransactionQueryDAO`는 DAO의 한 예로, 특정 쿼리를 수행하는 로직을 담고 있을 것입니다.

3. **Entity**: Entity는 데이터베이스의 테이블을 클래스로 표현한 것입니다. NestJS에서는 TypeORM과 같은 ORM 라이브러리를 사용하여 Entity를 정의할 수 있습니다. 각 인스턴스는 테이블의 레코드를 나타내며, 클래스의 프로퍼티는 테이블의 컬럼에 해당합니다.

```typescript
import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class Cat {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({ length: 500 })
  name: string;

  @Column("int")
  age: number;

  @Column({ length: 500 })
  breed: string;
}
```

이 세 가지 개념은 백엔드 개발에서 데이터를 처리하고 관리하는 데 중요한 역할을 합니다. DTO는 데이터의 전송과 유효성 검사를, DAO는 데이터베이스 접근 로직을, Entity는 데이터베이스의 테이블을 각각 캡슐화합니다.
