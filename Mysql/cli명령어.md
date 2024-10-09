sql 접속

mysql -u root -p

-> 비밀번호 입력

# 데이터베이스 조회

```sql
show database;
```

// 사용할 데이터베이스로 이동

```zsh
use ${데이터베이스명};
# Database changed
```

# 현재 선택된 데이터베이스 조회

```sql
select database();
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

1. **회원 정보 테이블 (Users):**

```sql
CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

2. **게시물 테이블 (Posts):**

```sql
CREATE TABLE Posts (
    post_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    title VARCHAR(255) NOT NULL,
    content TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE
);
```

3. **댓글 테이블 (Comments):**

```sql
CREATE TABLE Comments (
    comment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    post_id INT,
    content TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (post_id) REFERENCES Posts(post_id) ON DELETE CASCADE
);
```

이 스키마에서 사용자(User), 게시물(Post), 댓글(Comment) 테이블이 각각 정의되어 있습니다. 사용자 정보와 게시물은 각각의 테이블에 저장되고, 댓글은 게시물에 속하며 해당 게시물의 키를 참조합니다.

물론, 실제 애플리케이션의 요구사항에 따라 스키마를 조정해야 할 수 있습니다. 예를 들어, 보안 및 성능 향상을 위해 암호화, 인덱스, 그리고 필요한 유형의 제약 조건 등을 추가할 수 있습니다.
