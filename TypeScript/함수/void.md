# `handleButtonDisabled();`와 `void handleButtonDisabled();`의 차이점

`handleButtonDisabled();`와 `void handleButtonDisabled();`는 서로 다른 결과를 가져옵니다.

1. `handleButtonDisabled();`: 이것은 `handleButtonDisabled` 함수를 호출하고, 함수가 반환하는 값을 반환합니다. 만약 `handleButtonDisabled` 함수가 어떤 값을 반환한다면, 그 반환값이 사용됩니다.

2. `void handleButtonDisabled();`: 이것은 `handleButtonDisabled` 함수를 호출하지만, 반환값을 무시하고 항상 `undefined`를 반환합니다. `void` 키워드는 특별한 용도로 사용되며, 표현식의 결과를 무시하고 싶을 때 유용합니다.

보통 `void` 키워드는 일부 프레임워크나 라이브러리에서 특정 이벤트 핸들러 함수를 호출하고자 할 때 사용될 수 있습니다. 예를 들어, HTML에서 버튼 클릭 시 페이지 리로딩을 막기 위해 JavaScript 함수를 호출하는 경우에 사용될 수 있습니다:

```html
<button onclick="void handleButtonDisabled();">Click Me</button>
```

이렇게 하면 버튼 클릭 시 `handleButtonDisabled` 함수를 호출하지만, 해당 함수의 반환값을 무시하므로 페이지가 새로 고침되지 않습니다.

일반적으로 함수 호출을 위해서는 단순히 `handleButtonDisabled();`를 사용하고, 특별한 경우에 `void` 키워드를 사용하여 반환값을 무시할 수 있습니다.

## void ~ 와 await~ 비동기 함수 호출 시 차이점

`void deQueue(...)`와 `await deQueue(...)` 사이의 주요 차이점은 비동기 처리와 관련이 있습니다.

1. `void deQueue(...)`: 이 표현은 `deQueue(...)` 함수를 호출하고 해당 함수의 반환 값을 무시하며, 함수 호출이 완료되면 다음 코드로 진행합니다. 비동기 함수를 호출하더라도 해당 함수의 완료를 기다리지 않습니다. 이 경우 `await` 키워드가 없으므로 비동기 함수의 결과를 기다리지 않습니다.

2. `await deQueue(...)`: 이 표현은 `deQueue(...)` 함수를 호출하고 해당 함수의 반환 값을 기다립니다. 함수 호출이 완료될 때까지 다음 코드로 진행하지 않습니다. 이 경우 비동기 함수의 결과를 기다리면서, 해당 결과가 반환될 때까지 코드 실행이 일시 중단됩니다.

따라서 `await` 키워드를 사용하는 경우에는 비동기 함수가 완료되기를 기다리고, 그 결과를 사용할 수 있게 됩니다. 이와 달리 `void` 키워드를 사용하면 함수의 반환 값을 무시하고 비동기 함수의 완료 여부만 고려합니다.
