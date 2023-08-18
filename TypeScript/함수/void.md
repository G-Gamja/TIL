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
