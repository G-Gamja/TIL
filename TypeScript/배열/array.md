## 배열

## 배열 삭제
https://developer-talk.tistory.com/153
## 배열복사
https://yangheat.tistory.com/54

## filter method

조건에 따라 배열의 값들을 걸러주는 함수이다

map은 배열을 재가공한다는 느낌이기 때문에 단순히 splice등 배열 내의 특정 요소의 삭제등을 위해선 filter메서드가 더 합리적인 선택이다
```typescript
 const deleteMethod = (selectedindex: number) => {
    // userList.map((e, index) => (index === selectedindex ? userList.splice(index, 1) : null));

    덤으로 불필요한 계산 과정도 줄일 수 있다.
    // const copiedList = [...userList]
    const copiedList = userList.filter((e, index) => index !== selectedindex);
    setUserList(copiedList);
    window.localStorage.setItem('account', JSON.stringify(userList));
  };
  ```
# 배열을 string으로 변환
`arr.join(separator) `

join() 함수는 배열의 모든 값들을 연결한 문자열을 리턴합니다.

이때 각각의 값들 사이에는 파라미터로 입력된 구분자(separator)가 들어가게 됩니다.

만약, separator를 입력하지 않은 경우, default로 ','가 들어갑니다.

예시:  `return squidRoute.error.errors.map(({message})=>message).join('\n');`

참조: https://hianna.tistory.com/447

# 배열 비교

- 1차원 배열 비교, 순서가 뒤죽박죽이지만 내용만 같은지를 비교할 때
```ts
accounts.length === revertedAccount.length && accounts.every((account, idx) => account === revertedAccount[idx]),

```

```ts
JSON.stringify(accounts) === JSON.stringify(revertedAccount)
```
sort메서드는 호출한 객체의 데이터를 직접 조작하기 때문에 복사한 값을 사용할 것

```ts
JSON.stringify(
                  [...accounts].sort((a, b) => {
                    if (a.id > b.id) return 1;
                    if (a.id < b.id) return -1;
                    return 0;
                  }),
                ) ===
                  JSON.stringify(
                    [...revertedAccount].sort((a, b) => {
                      if (a.id > b.id) return 1;
                      if (a.id < b.id) return -1;
                      return 0;
                    }),
                  ),
```
비교용 리스트
```ts
              const testList = revertedAccount.map((item) => ({
                ...item,
                type: 'ahn',
              }));
```

# 중복제거

```ts
const origins = Array.from(new Set(allowedOrigins.map((item) => item.origin)));

```


# 배열 합산

```ts
  const delegationAmount = useMemo(
    () =>
      delegation?.data
        ?.filter((item) => item.amount?.denom === chain.baseDenom)
        ?.reduce((ac, cu) => plus(ac, cu.amount.amount), '0') // 여기서 합산
        .toString() || '0',
    [chain.baseDenom, delegation?.data],
  );
```