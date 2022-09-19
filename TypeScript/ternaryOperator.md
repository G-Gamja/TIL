# 삼항연산자
https://learnjs.vlpt.us/useful/01-ternary.html

`조건 ? true일때 : false일때`

```typescript
두개가 상동한 기능을 가짐
const array = [];
let text = '';
if (array.length === 0) {
  text = '배열이 비어있습니다.';
} else {
  text = '배열이 비어있지 않습니다.';
}
console.log(text);

const array = [];
let text = array.length === 0 ? '배열이 비어있습니다' : '배열이 비어있지 않습니다.';

console.log(text);
```

```typescript
두개가 상동한 기능을 가짐
     let userDataSet: userDataSetModel[];
    // ** initialize user data set with nullsafety */
    if (fetchedData) {
       userDataSet = JSON.parse(fetchedData);
     } else {
      userDataSet = [];
     }


    const userDataSet: userDataSetModel[] = fetchedData ? JSON.parse(fetchedData) : [];
```

## 삼항연산자의 중첩사용
```typescript
const condition1 = false;
const condition2 = false;

const value = condition1 
  ? '와우!' 
  : condition2 
    ? 'blabla' 
    : 'foo';

console.log(value);
```
