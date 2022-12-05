https://radlohead.gitbook.io/typescript-deep-dive/future-javascript/spread-operator

https://fenderist.tistory.com/401
# 오브젝트 스프레드

```typescript
const point2D = { x: 1, y: 2 }
/** Create a new object by using all the point2D props along with z */
const point3D = { ...point2D, z: 3 }
//{ x: 1, y: 2, z: 3}
```

같은 필드를 선언시 후에 선언된 것이 오버라이드 된다
```typescript
const point2D = { x: 1, y: 2 }
const anotherPoint3D = { x: 5, z: 4, ...point2D }
console.log(anotherPoint3D) // {x: 1, y: 2, z: 4}
const yetAnotherPoint3D = { ...point2D, x: 5, z: 4 }
console.log(yetAnotherPoint3D) // {x: 5, y: 2, z: 4}

```
파라미터에도 정의가 가능하다
```typescript
const add = (...nums: number[]) => {
    return nums.reduce((r, b) => {
        return r + b;
    }, 0);
}

const addNums = add(5, 6, 7, 8);

console.log(addNums)  -> 26
```


나머지는 리스트로 넣어버리기
``` typescript
const a = ['a', 'b', 'c']

const z = ['z']

z.push(...a)

const [i, j, ...rest] = z;

console.log(i, j, rest);
// [LOG]: "z",  "a",  ["b", "c"] 
```
