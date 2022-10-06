https://kyounghwan01.github.io/blog/TS/fundamentals/utility-types/#partial
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  brand: string;
  stock: number;
}
// shoppingItem 타입은 Product타입의 필드 중에서 우측 3개만 뽑아서 정의하겠다는 의미임
type shoppingItem = Pick<Product, "id" | "name" | "price">;

// 상품의 상세정보 (Product의 일부 속성만 가져온다)
function displayProductDetail(shoppingItem: shoppingItem) {
  // id, name, price의 일부만 사용 or 별도의 속성이 추가되는 경우가 있음
  // 인터페이스의 모양이 달라질 수 있음
}
```