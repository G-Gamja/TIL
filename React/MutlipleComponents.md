## 컴포넌트 여러개 반복 렌더링 하는법 

https://goddaehee.tistory.com/303
```typescript
  const userNameListView = userList.map((e, index) => (
    <AccountContainer key={index} mainText={e.name} onClickEdit={() => onClickEdit(index, e.name)} onClickDelete={() => onDelete(index)} />
  ));

  <div className={styles.mainField}>
          <div className={styles.ListView}>{userNameListView}</div>
          <SubmitButton mainText="Create Account" />
        </div>
  ```

