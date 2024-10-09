https://futurists.tistory.com/11

# ALTER

```zsh
mysql> alter table Posts add FOREIGN KEY(user_id) REFERENCES Users(id);
```

# JOIN

## 내부 조인

```zsh
mysql> select * from Posts join Users on Posts.user_id = Users.id;
```

각 테이블의 특정 컬럼만 가져오기

```zsh
    const joinedPosts = await this.knex.raw(
      `SELECT Users.username, Posts.title, Posts.content
      FROM Users
      JOIN Posts ON Users.id = Posts.user_id`,
    );
```
