# Warning: validateDOMNesting(...): <button> cannot appear as a descendant of <button>

문제 원인

- 버튼 컴포넌트의 children중 버튼이 또 있는 현상 (이중 버튼)

예시

```tsx
    <ValidatorButton
      data-is-disabled={isUndelegate}
      onClick={() => {
        if (!isUndelegate) {
          onClickValidator(true);
        }
      }}
    >
            <LeftColumnTitleIconButton
              onClick={(e) => {
                e.stopPropagation();
                window.open(`https://www.mintscan.io/dydx/validators/${validator?.operator_address || ''}`);
              }}
            >
              <MintscanButtonIcon />
      </LeftContainer>

      {!isUndelegate && (
        <RightContainer>
          <RightChevronIcon />
        </RightContainer>
      )}
    </ValidatorButton>
```

답: If the button is having one more button as a child then we recive this error, if the parent component is having button property remove the child button

해결책:

- 부모 버튼을 'button'이 아닌 'div'로 변경해버리자

참고 : https://stackoverflow.com/questions/61341666/validatedomnesting-button-cannot-appear-as-a-descendant-of-button
