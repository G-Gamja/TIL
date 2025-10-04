1.이벤트 버블링 공부하기

2. 함수 앞 void의 의미
3. 캐싱 컨트롤
4. axios header 커스텀
5. rebase
6.
7. promise, promise.all, promise.allSettled
8. Router
9. aws cloudfront ,CI CD
10. index.html이 하는 역할이 뭔지

    <meta property="og:url" content="https://dash.splash.im/" />
    <meta property="og:type" content="Sui dApp by Cosmostation" />
    <meta property="og:title" content="Splash - Sui Staking dApp" />

11. await와 try catch 문 둘 다 관련성 확인해볼것
    - await와 void 차이점
12. 한 훅 내에서 다른 훅을 사용한 값을 사용할 때 쓰이는 훅에 서스펜스를 걸어야하나 안하나...
13. headless component, radix ui, shacdn ui, 컴파운드 컴포넌트 패턴

웹서버 - was 리서치

오 👌 딱 좋은 방향이에요.
다만 키워드를 조금만 조정하면 훨씬 더 많은 자료를 찾을 수 있어요.

---

## 📌 검색 키워드 추천

1. **웹서버 아키텍처**
   → 웹서버와 WAS, DB가 어떻게 연결되는지 큰 그림 설명 자료 찾기 좋음

2. **3-tier architecture (3계층 아키텍처)**
   → 클라이언트(프론트) / 웹서버(리버스 프록시) / 애플리케이션 서버 / DB 구조를 설명한 고전적이지만 기본이 되는 모델

3. **Nginx reverse proxy architecture**
   → 실제 배포에서 가장 많이 쓰이는 구조 (정적 파일 + API 라우팅)

4. **React deployment architecture / React Nginx deployment**
   → React 프로젝트를 어떻게 웹서버(Nginx)로 배포하는지 구체적인 예시 찾을 수 있음

5. **NestJS deployment architecture**
   → 백엔드까지 같이 붙여서 Nginx 앞단 + NestJS 뒤단 구조 설명

---

## 📌 공부할 때 보면 좋은 흐름

- 웹서버(Web Server)와 WAS(Web Application Server) 개념
- **Reverse Proxy / Load Balancer** (Nginx, HAProxy 등)
- **정적 파일 배포** (React build → Nginx 서빙)
- **백엔드 연동** (NestJS 같은 API 서버 + Nginx 라우팅)
- 더 나아가면: **Docker 기반 배포, CI/CD, Cloud (AWS S3+CloudFront, ECS, EKS)**

---

👉 그러니까, 단순히 “웹서버 아키텍처”라고 검색해도 되지만,
**“웹 애플리케이션 아키텍처”** 또는 **“3-tier architecture”** 라고 검색하면 훨씬 풍부한 자료를 볼 수 있어요.

---

원하면 제가 **React + NestJS + Nginx + DB** 기본 아키텍처 다이어그램 하나 그려드릴까요?
