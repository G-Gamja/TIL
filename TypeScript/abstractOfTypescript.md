## 구조적 타입 시스템

TypeScript의 핵심 원칙 중 하나는 타입 검사가 값이 있는 형태에 집중한다는 것입니다. 이는 때때로 "덕 타이핑(duck typing)" 또는 "구조적 타이핑" 이라고 불립니다.

`구조적 타입 시스템`에서 두 객체가 같은 형태를 가지면 같은 것으로 간주됩니다.

```typescript
interface Point {
  x: number;
  y: number;
}

function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
//point가 Point타입으로 선언된 적이 없음
// "12, 26"를 출력합니다
const point = { x: 12, y: 26 };
printPoint(point);
```
`point`변수는 `Point`타입으로 선언된 적이 없지만, TypeScript는 타입 검사에서 `point`의 형태와 `Point`의 형태를 비교합니다. 둘 다 같은 형태이기 때문에, 통과합니다.

형태 일치에는 일치시킬 객체의 필드의 하위 집합만 필요합니다.

```typescript
// @errors: 2345
interface Point {
  x: number;
  y: number;
}

function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
// ---cut---
const point3 = { x: 12, y: 26, z: 89 };
printPoint(point3); // prints "12, 26"

const rect = { x: 33, y: 3, width: 30, height: 80 };
printPoint(rect); // prints "33, 3"

const color = { hex: "#187ABF" };

printPoint(color);
```

객체 또는 클래스에 필요한 모든 속성이 존재한다면, TypeScript는 구현 세부 정보에 관계없이 일치하게 봅니다.
```typescript
// @errors: 2345
interface Point {
  x: number;
  y: number;
}

function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
// ---cut---
class VirtualPoint {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const newVPoint = new VirtualPoint(13, 56);
printPoint(newVPoint); // prints "13, 56"```