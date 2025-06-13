### MobX와 `autorun` 설명

MobX는 한마디로 **"상태 관리 라이브러리"** 입니다. React나 Vue 같은 프레임워크에서 컴포넌트의 상태를 효율적으로 관리하고, 상태가 변경될 때 UI를 자동으로 업데이트하는 데 도움을 줍니다. 마치 엑셀 스프레드시트처럼요.

**MobX의 핵심 개념:**

1.  **Observable (관찰 가능한 상태):**

    - MobX의 핵심은 **`observable`** 입니다. JavaScript의 기본 데이터 타입 (객체, 배열, Map 등)을 `observable`로 만들면, MobX가 이 데이터의 변화를 "감지"할 수 있게 됩니다.
    - 예를 들어, `user.name`이라는 `observable` 변수가 있다면, 이 `user.name`의 값이 변경될 때 MobX는 그 변화를 알아챕니다.
    - **비유:** 엑셀 시트에서 특정 셀에 `A1`이라는 이름표를 붙여놓고, `A1`의 값이 바뀔 때마다 누군가에게 알려주는 것과 같습니다.

2.  **Action (상태 변경):**

    - `observable`한 상태는 직접 변경하지 않고, **`action`** 내부에서 변경하는 것을 권장합니다. `action`은 상태 변경이 발생하는 "의도"를 명확히 하고, MobX가 상태 변경을 일괄적으로 처리하여 성능 최적화를 돕습니다.
    - **비유:** 엑셀 시트에서 특정 셀의 값을 바꿀 때, "이 셀의 값을 바꾸겠습니다!"라고 선언하고 바꾸는 것과 같습니다.

3.  **Reaction (반응):**
    - `observable`한 상태가 변경되었을 때, 특정 "행위"를 하도록 만드는 것이 바로 **`reaction`** 입니다. `reaction`에는 여러 종류가 있는데, `autorun`이 대표적입니다.
    - **비유:** 엑셀 시트에서 `A1` 셀의 값이 바뀌었을 때, `B1` 셀의 값을 자동으로 계산하거나, 특정 그래프를 업데이트하는 것과 같습니다.

---

### `autorun` 자세히 살펴보기

`autorun`은 MobX의 `reaction` 중 가장 기본적인 형태입니다.

**`autorun`의 작동 방식:**

1.  **`autorun` 함수 실행:** `autorun`에 콜백 함수를 전달합니다.
2.  **의존성 추적:** 이 콜백 함수가 실행되는 동안 **접근하는 모든 `observable` 상태**를 MobX가 자동으로 "추적"합니다. 즉, 이 콜백 함수가 어떤 `observable` 데이터에 의존하는지를 MobX가 알아챕니다.
3.  **자동 재실행:** 추적하고 있던 `observable` 상태 중 **어느 하나라도 변경되면**, `autorun`에 전달했던 콜백 함수가 **자동으로 다시 실행**됩니다.

**언제 `autorun`을 사용할까요?**

- **디버깅:** 상태 변화를 콘솔에 로깅하고 싶을 때.
- **사이드 이펙트:** UI 업데이트 외의 다른 부수 효과 (예: 서버로 데이터 전송, 로컬 스토리지 저장)가 필요할 때.
- **특정 계산 값:** 여러 `observable` 값을 조합하여 새로운 계산 값을 얻고, 이 값이 변경될 때마다 특정 로직을 실행하고 싶을 때 (이 경우 `computed`와 함께 사용될 수도 있습니다).
- **React 컴포넌트 외부:** React 컴포넌트의 `render` 메서드 내부에서 상태 변화에 반응하여 UI를 업데이트하는 것은 MobX-React의 `@observer` 데코레이터가 처리합니다. `autorun`은 주로 React 컴포넌트 외부에서, 특정 상태 변화에 따라 다른 작업을 수행해야 할 때 유용합니다.

**코드 예시:**

```javascript
import { makeObservable, observable, action, autorun } from 'mobx';

class CounterStore {
  // observable로 만들어서 MobX가 변화를 감지하도록 합니다.
  @observable count = 0;
  @observable message = 'Hello MobX';

  constructor() {
    // MobX 6 이상에서는 makeObservable을 사용해야 합니다.
    makeObservable(this);
  }

  // action으로 감싸서 상태 변경의 의도를 명확히 합니다.
  @action
  increment() {
    this.count++;
  }

  @action
  setMessage(newMessage) {
    this.message = newMessage;
  }
}

const counter = new CounterStore();

// autorun 예시 1: count 변화를 감지하고 콘솔에 출력
autorun(() => {
  console.log(`현재 카운트: ${counter.count}`);
});

// autorun 예시 2: message 변화를 감지하고 콘솔에 출력
autorun(() => {
  console.log(`현재 메시지: ${counter.message}`);
});

// autorun 예시 3: count와 message 둘 다 변경되면 실행
autorun(() => {
  console.log(`카운트와 메시지: ${counter.count}, ${counter.message}`);
});

console.log('--- 초기 실행 ---');
// 초기 실행: autorun은 초기 한 번 실행됩니다.
// 출력:
// 현재 카운트: 0
// 현재 메시지: Hello MobX
// 카운트와 메시지: 0, Hello MobX

console.log('--- count 증가 ---');
counter.increment();
// 출력:
// 현재 카운트: 1
// 카운트와 메시지: 1, Hello MobX
// (message는 변하지 않았으므로 두 번째 autorun은 실행되지 않습니다)

console.log('--- message 변경 ---');
counter.setMessage('Bye MobX');
// 출력:
// 현재 메시지: Bye MobX
// 카운트와 메시지: 1, Bye MobX
// (count는 변하지 않았으므로 첫 번째 autorun은 실행되지 않습니다)
```

**핵심 포인트:**

- `autorun`은 내부에서 **접근하는 `observable` 값에만 반응**합니다. `autorun` 함수 내에서 `counter.count`에 접근하지 않았다면, `counter.count`가 변해도 해당 `autorun`은 재실행되지 않습니다.
- `autorun`은 **초기 한 번 실행**됩니다.
- `autorun`은 `dispose` 함수를 반환하며, 이 함수를 호출하면 더 이상 `autorun`이 작동하지 않도록 중지할 수 있습니다. (메모리 누수 방지 등을 위해 컴포넌트가 언마운트될 때 등 필요에 따라 호출)

```javascript
const disposer = autorun(() => {
  console.log(`이 autorun은 곧 중지됩니다: ${counter.count}`);
});

counter.increment(); // 실행됨
counter.increment(); // 실행됨

disposer(); // autorun 중지

counter.increment(); // 더 이상 실행되지 않음
```

---

**MobX와 React의 관계 (간단히):**

MobX는 상태 관리 라이브러리고, React는 UI 렌더링 라이브러리입니다. 이 둘은 서로 다른 역할을 하지만, 함께 사용할 때 시너지가 좋습니다.

- **`@observer` (mobx-react-lite 또는 mobx-react):** React 컴포넌트를 `observer`로 감싸면, 해당 컴포넌트가 사용하는 `observable` 상태가 변경될 때 **해당 컴포넌트만** 자동으로 리렌더링됩니다. `autorun`과 비슷하지만, `autorun`은 UI 렌더링 외의 일반적인 반응에 사용하고, `@observer`는 React 컴포넌트의 렌더링에 특화되어 사용한다고 생각하시면 됩니다.

---

### 결론적으로

MobX는 `observable`한 상태를 만들고, 이 상태가 `action`을 통해 변경되면 `reaction` (예: `autorun`, `@observer`)이 자동으로 반응하여 필요한 작업을 수행하는 방식입니다.

`autorun`은 여러분이 지정한 `observable` 상태가 변경될 때마다 특정 코드를 자동으로 실행시키고 싶을 때 사용하는 MobX의 강력한 기능입니다. React 컴포넌트의 렌더링과는 별개로, 상태 변화에 따른 부수 효과를 처리하는 데 특히 유용합니다.
