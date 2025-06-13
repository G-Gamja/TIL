---
# TIL: Windowing (가상 스크롤링) in React

## ✨ 개요

오늘은 리스트 렌더링 성능 최적화를 위해 **Windowing** 기법을 학습했다.
특히 React에서 `filteredAssetsBySearch.map(...)`과 같은 패턴으로 많은 데이터를 한꺼번에 렌더링할 때, 브라우저 성능 이슈가 발생할 수 있다.

이를 해결하기 위해 **가상 스크롤링(Virtual Scroll)** 또는 **Windowing**이라는 기술이 사용된다.
---

## 🧠 문제 상황

- 배열 크기가 클수록 map으로 모든 항목을 한꺼번에 렌더링 → **DOM 부하 증가**
- 렉 걸림, 스크롤 시 버벅임
- 실제로는 화면에 **보이는 일부 요소만 렌더링해도 충분**함

---

## ✅ 해결 방법: Windowing

### 개념

> 화면에 보이는 아이템만 렌더링하고, 나머지는 렌더링하지 않는다.

이 기법을 통해 렌더링 비용을 크게 줄이고 UX 성능을 개선할 수 있다.

---

## 🔧 사용 도구

### `react-window`

- Lightweight & performant
- 기본적인 vertical list에 적합
- `FixedSizeList`, `VariableSizeList` 등 제공

```bash
yarn add react-window
```

### 예제 코드

```tsx
import { FixedSizeList as List } from 'react-window';

<List
  height={600}
  itemCount={filteredAssetsBySearch.length}
  itemSize={72} // 한 항목의 높이(px)
  width="100%"
>
  {({ index, style }) => {
    const coin = filteredAssetsBySearch[index];
    return (
      <div style={style}>
        <CoinWithMarketTrendButton ... />
      </div>
    );
  }}
</List>
```

> `style={style}`은 필수! → 위치 계산 및 스크롤 위치 동기화에 필요

---

## 💡 배운 점

- 데이터가 많을수록 `map()` 방식의 렌더링은 치명적일 수 있다.
- `react-window`를 사용하면 렌더링 성능을 **극적으로 향상**시킬 수 있다.
- IntersectionObserver + Lazy Load는 보완적이지만, 가상 스크롤이 더 근본적 해결책이다.

---

## 📚 참고 링크

- [react-window GitHub](https://github.com/bvaughn/react-window)
- [react-virtual (TanStack)](https://tanstack.com/virtual/v3)

---
