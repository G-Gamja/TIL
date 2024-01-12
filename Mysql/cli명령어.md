sql 접속

mysql -u root -p

-> 비밀번호 입력

// 사용할 데이터베이스로 이동

```zsh
USE ${데이터베이스명};
Database changed
```

# 테이블 생성

```sql
CREATE TABLE your_table_name (
    column1 INT PRIMARY KEY,
    column2 VARCHAR(255),
    created_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    other_fk INT,
    FOREIGN KEY (other_fk) REFERENCES test_b(id)
);
```

# 프라이머리 키 생성 문법

```sql
CREATE TABLE tablename
(
   column1 datatype NOT NULL,
   column2 datatype NOT NULL,
    ...
    CONSTRAINT contraint_name,             #ex> CONSTRAINT PK_person, 생략시 자동생성
    PRIMARY KEY(column1, column2, ...) #여러개 지정 가능
);
```

예시

```sql
CREATE TABLE test_user (
  id INT NOT NULL,
  username VARCHAR(16) NOT NULL,
  email VARCHAR(255) NULL,
  password VARCHAR(32) NOT NULL,
  create_time TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id));


=> id를 기본키로 지정하는 예제
=> CONSTRAINT 생략가능
=> id 옆에 PRI 표시가 됨
=> desc test_user; // 테이블 정보 확인

mysql> desc person;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| pid   | int         | NO   | PRI | NULL    |       |
| name  | varchar(16) | YES  |     | NULL    |       |
| age   | int         | YES  |     | NULL    |       |
| sex   | char(1)     | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.03 sec)
```

프라이머리 키로 여러 열을 지정할 수 있음

```sql
CREATE TABLE test_user (
  id INT NOT NULL,
  username VARCHAR(16) NOT NULL,
  email VARCHAR(255) NULL,
  password VARCHAR(32) NOT NULL,
  create_time TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id, username));
```

참조: https://velog.io/@jane15/PRIMARY-KEY-FOREIGN-KEY
