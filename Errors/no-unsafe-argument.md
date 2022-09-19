## no-unsafe-assignment
https://typescript-eslint.io/rules/no-unsafe-assignment/
Unsafe argument of type `any` assigned to a parameter of type
```typescript

  type userDataSetModel = { name: string };
  const fetchedData = window.localStorage.getItem('account');
  // 초기화할때 밑에 json parse할떄 그거를 여따가 이식하면 되겠다
  const [userList, setUserList] = useState<userDataSetModel>(fetchedData ? JSON.parse(fetchedData) : []);//여기서 해당에러가 발생함
  ```