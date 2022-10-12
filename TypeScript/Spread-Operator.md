https://radlohead.gitbook.io/typescript-deep-dive/future-javascript/spread-operator

# 오브젝트 스프레드

```typescript
const point2D = { x: 1, y: 2 }
/** Create a new object by using all the point2D props along with z */
const point3D = { ...point2D, z: 3 }
```

같은 필드를 선언시 후에 선언된 것이 오버라이드 된다
```typescript
const point2D = { x: 1, y: 2 }
const anotherPoint3D = { x: 5, z: 4, ...point2D }
console.log(anotherPoint3D) // {x: 1, y: 2, z: 4}
const yetAnotherPoint3D = { ...point2D, x: 5, z: 4 }
console.log(yetAnotherPoint3D) // {x: 5, y: 2, z: 4}

```