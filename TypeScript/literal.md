## 리터럴 방식

https://velog.io/@muz/TypeScript-%EB%A6%AC%ED%84%B0%EB%9F%B4-%ED%83%80%EC%9E%85

```typescript
type Easing = "ease-in" | "ease-out" | "ease-in-out";

class UIElement{
	animate(dx: number, dy: number, easing: Easing) {
    	if(easing == "ease-in") {
        	//...
        } else if (easing === "ease-out") {
        } else if (easing === "ease-in-out") {
        } else {
        	// 누군가가 타입을 무시하면 else 구문이 실행됨
        }
    }
}
