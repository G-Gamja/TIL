https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-Passport-%EB%AA%A8%EB%93%88-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%B2%98%EB%A6%AC%EA%B3%BC%EC%A0%95-%F0%9F%92%AF-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90

# providers에 주입된 커스텀 Strategy를 AuthGuard에서는 어떻게 찾는 걸까?

https://jay-ji.tistory.com/94

# 쿠키에 저장된 토큰값 Request에서 가져오는법

기존 방식 처럼 `jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken()`을 사용하면 헤더에 Bearer 토큰을 넣어야 하는데, 쿠키에 넣어서 사용하고 싶다면 아래와 같이 사용하면 된다.

```ts

      jwtFromRequest: ExtractJwt.fromExtractors([
        (request) => {
          return request?.cookies?.Authorization;
        },
      ]),
```

https://sound-story.tistory.com/19
