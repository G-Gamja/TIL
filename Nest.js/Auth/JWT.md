# JWT인증 방식 플로우

JWT (JSON Web Token) 인증 과정은 다음과 같은 단계로 이루어집니다. 클라이언트가 서버에 인증을 요청하고, 서버가 인증된 토큰을 발급하여 클라이언트가 이를 사용해 인증된 요청을 보낼 수 있게 하는 절차입니다.

### 1. 사용자 로그인

1. **사용자 로그인 요청**:
   - 사용자는 로그인 페이지에서 아이디와 비밀번호를 입력하고 로그인 요청을 보냅니다.
   - 예를 들어, 클라이언트가 `/login` 엔드포인트에 POST 요청을 보냅니다.

```javascript
// 예시: 로그인 요청
const response = await axios.post("http://your-server.com/login", {
  username: "exampleUser",
  password: "examplePassword",
});
```

### 2. 서버에서 사용자 인증

2. **사용자 인증 확인**:
   - 서버는 받은 아이디와 비밀번호를 데이터베이스에서 조회하여 확인합니다.
   - 인증이 성공하면, 서버는 사용자 정보를 기반으로 JWT를 생성합니다.

```javascript
// 서버 예시: 사용자 인증 및 JWT 생성
app.post("/login", async (req, res) => {
  const { username, password } = req.body;
  // 데이터베이스에서 사용자 확인 (예시)
  const user = await findUserInDatabase(username, password);

  if (!user) {
    return res.status(401).send("Invalid credentials");
  }

  // JWT 생성
  const token = jwt.sign(
    { userId: user.id, username: user.username },
    process.env.JWT_SECRET,
    { expiresIn: "1h" }
  );

  // JWT 토큰을 클라이언트에 반환
  res.json({ token });
});
```

### 3. 클라이언트에서 JWT 저장

3. **클라이언트에서 JWT 저장**:
   - 클라이언트는 서버로부터 받은 JWT를 로컬 스토리지나 쿠키에 저장합니다.
   - 이후 클라이언트는 이 토큰을 인증된 요청을 보낼 때 사용합니다.

```javascript
// 클라이언트 예시: JWT 저장
localStorage.setItem("jwtToken", response.data.token);
```

### 4. 인증된 요청 보내기

4. **인증된 요청 보내기**:
   - 클라이언트는 보호된 엔드포인트에 요청을 보낼 때 저장된 JWT를 헤더에 포함하여 보냅니다.
   - 서버는 이 JWT를 확인하여 요청이 인증된 요청인지 판단합니다.

```javascript
// 클라이언트 예시: 인증된 요청 보내기
const token = localStorage.getItem("jwtToken");

const apiResponse = await axios.get(
  "http://your-server.com/protected-endpoint",
  {
    headers: {
      Authorization: `Bearer ${token}`,
    },
  }
);
```

### 5. 서버에서 JWT 검증

5. **서버에서 JWT 검증**:
   - 서버는 요청 헤더에 포함된 JWT를 확인하고, 유효한지 검증합니다.
   - JWT가 유효하면 요청을 처리하고, 유효하지 않으면 401 Unauthorized 응답을 보냅니다.

```javascript
// 서버 예시: JWT 검증 미들웨어
function authenticateToken(req, res, next) {
  const authHeader = req.headers["authorization"];
  const token = authHeader && authHeader.split(" ")[1];

  if (!token) return res.sendStatus(401);

  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);

    req.user = user;
    next();
  });
}

// 보호된 엔드포인트
app.get("/protected-endpoint", authenticateToken, (req, res) => {
  res.send("This is a protected endpoint");
});
```

### 요약

1. **사용자 로그인 요청**: 사용자가 로그인 요청을 보냅니다.
2. **서버에서 사용자 인증**: 서버가 아이디와 비밀번호를 확인하고, 성공 시 JWT를 생성합니다.
3. **클라이언트에서 JWT 저장**: 클라이언트가 JWT를 로컬 스토리지나 쿠키에 저장합니다.
4. **인증된 요청 보내기**: 클라이언트가 보호된 엔드포인트에 요청할 때 JWT를 포함하여 보냅니다.
5. **서버에서 JWT 검증**: 서버가 JWT를 검증하고, 유효한 요청인지 확인합니다.

이 과정을 통해 JWT를 사용하여 안전하게 사용자 인증을 처리할 수 있습니다.

# 갱신 전략

![리다이렉트](/images/jwt-refresh.png)

1. Access Token의 유효 기간을 짧게 설정, Refresh Token의 유효 기간은 길게 설정
2. 두 토큰 모두 서버에 전송하여 Access Token으로 인증하고, 만료 시 Refresh Token으로 재발급한다.
3. Access Token을 탈취당해도, 짧은 유효 기간이 지나면 사용할 수 없다.

자세한 참고문헌: https://velog.io/@from_numpy/NestJS-How-to-implement-Refresh-Token-with-JWT

살짝 가볍게 읽기 편한: https://mag1c.tistory.com/485

읽기 편한데 설명 자세함, 내가 짠 코드랑 유사: https://soonyubi.github.io/jwt-refresh-token/

리프레시 토큰을 유저 테이블의 특정 컬럼에 저장해야함

1. 로그인 시 엑세스 토큰, 리프레시 토큰을 생성
2. 생성한 리프레시 토큰은 해싱 후 DB에 저장
3. 엑세스 토큰, 리프래시 토큰을 클라이언트에게 전달(셋쿠키)
