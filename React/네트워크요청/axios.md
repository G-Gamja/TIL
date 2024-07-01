# axios

## 엑시오스 요청 취소하기

```ts
import axios from "axios";

const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios
  .get("https://jsonplaceholder.typicode.com/posts", {
    cancelToken: source.token,
  })
  .then((response) => {
    console.log(response);
  })
  .catch((error) => {
    if (axios.isCancel(error)) {
      console.log("Request canceled", error.message);
    } else {
      console.log("Request failed", error.message);
    }
  });

// 취소 요청
source.cancel("Operation canceled by the user.");
```

```ts
// 다른 예시

import axios from "axios";

// 요청 취소용 CancelToken 생성
const source = axios.CancelToken.source();

// Axios 요청 설정 객체에 cancelToken 속성 추가
axios
  .get("/api/data", { cancelToken: source.token })
  .then((response) => {
    // 요청이 성공적으로 완료되었을 때 처리할 로직
    console.log(response.data);
  })
  .catch((error) => {
    // 요청이 취소되었거나 실패했을 때 처리할 로직
    if (axios.isCancel(error)) {
      console.log("요청이 취소되었습니다:", error.message);
    } else {
      console.error("요청 실패:", error);
    }
  });

// 요청 취소
source.cancel("사용자에 의해 요청이 취소되었습니다.");
```
