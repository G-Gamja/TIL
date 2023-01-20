# 1. Failed prop type: Invalid prop children supplied to ForwardRef(Tooltip). Expected an element that can hold a ref

Tooltip태그 안에 바로 버튼 태그를 넣었을 시 발생하는 오류로 
뭐 ref관련 태그 안에는 

```tsx
<!-- "Did you accidentally use a plain function component for an element instead?" -->

<Tooltip>
  <NewFunctionalComponent />
</Tooltip>

<!-- Wrapped in a new div, devtools won't complain anymore -->

<Tooltip>
  <div>
    <NewFunctionalComponent />
  </div>
</Tooltip>

<!-- No more warnings! -->
```

오류 이유 참조: https://github.com/mui/material-ui/issues/31261
해결 방법 참조1: https://stackoverflow.com/questions/56347839/material-ui-v4-0-1-warning-expected-an-element-that-can-hold-a-ref
참조2: https://deepscan.io/docs/rules/react-func-component-invalid-ref-prop