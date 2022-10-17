하지만 컴포넌트를 아래와 같이 Suspense로 감싸주면 컴포넌트의 랜더링을 특정 작업 이후로 미루고, 그 작업이 끝날 때 까지는 fallback 속성으로 넘긴 컴포넌트를 대신 보여줄 수 있어요.

```typescript
<Suspense fallback={<Spinner />}>
  <UserList />
</Suspense>
```

참조: https://www.daleseo.com/react-suspense/