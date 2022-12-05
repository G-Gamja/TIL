# useSWR 훅이란

데이터를 가져오는 axios의 get같은 메서드인데 설정된 주기값으로 값을 알아서
다시 가져오기 때문에 편리하다
# Option - Suspense
항상 데이터를 가져오게 되며 undefine처리를 할 필요가 없지만, 에러 바운더리는 설정을 해줘야 데이터 못가져왔을때 에러처리가 된다  
가져온 데이터가 항상 렌더링 될 준비 (undefine의 가능성이 없다는 소리)가 되어있다. 
In Suspense mode, data is always the fetch response (so you don't need to check if it's undefined). But if an error occurred, you need to use an error boundary to catch it:

Normally, when you enabled suspense it's guaranteed that data will always be ready on render:

참조: https://swr.vercel.app/docs/suspense