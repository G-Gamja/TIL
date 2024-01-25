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
