# Strict 모드
StrictMode는 애플리케이션 내의 잠재적인 문제를 알아내기 위한 도구입니다. Fragment와 같이 UI를 렌더링하지 않으며, 자손들에 대한 부가적인 검사와 경고를 활성화합니다.

- 주의

Strict 모드는 개발 모드에서만 활성화되기 때문에, 프로덕션 빌드에는 영향을 끼치지 않습니다.
```tsx
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```
위의 예시에서, Header와 Footer 컴포넌트는 Strict 모드 검사가 이루어지지 않습니다. 하지만, ComponentOne과 ComponentTwo는 각각의 자손까지 검사가 이루어집니다.

StrictMode는 아래와 같은 부분에서 도움이 됩니다.

- 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
- 레거시 문자열 ref 사용에 대한 경고
- 권장되지 않는 findDOMNode 사용에 대한 경고
- 예상치 못한 부작용 검사
- 레거시 context API 검사
- Ensuring reusable state

참조: https://ko.reactjs.org/docs/strict-mode.html