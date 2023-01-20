하지만 컴포넌트를 아래와 같이 Suspense로 감싸주면 컴포넌트의 랜더링을 특정 작업 이후로 미루고, 그 작업이 끝날 때 까지는 fallback 속성으로 넘긴 컴포넌트를 대신 보여줄 수 있어요.



```typescript
<Suspense fallback={<Spinner />}>
  <UserList />
</Suspense>
```

근데 에러 바운더리는 무엇이냐하면 이제 데이터를 못가져올 떄, 가령 인터넷이 끊기거나 할 때 에러 바운더리가 보여지는 것이고 로딩중, 가져오는 중이면 서스펜스를 보여준다.
```ts
<ErrorBoundary fallback={<h2>Could not fetch posts.</h2>}>
  <Suspense fallback={<h1>Loading posts...</h1>}>
    <Profile />
  </Suspense>
</ErrorBoundary>
```
참조: https://www.daleseo.com/react-suspense/