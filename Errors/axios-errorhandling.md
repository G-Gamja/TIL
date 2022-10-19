So far, I cant able to get the error code like 404,500.., Still I am getting Network Error. How to solve this?
instance.post('/foo', {request_id : 12345})
.then(function(response) {})
.catch(function(error){
console.log(error); // Network Error
console.log(error.status); // undefined
console.log(error.code); // undefined
});


When error is "network error" that means Axios coudn't connect to your server, or for some reason does not get the response from your server. So that 401 error is never making it into Axios. Maybe post a question with some sample code on StackOverflow?

결론: No real solution. As I mentioned in my comment above, the actual error isn't forwarded to javascript by the browser. Its an issue with how browsers make requests and report errors, not with axios itself.

axios의 문제가 아님 cors정책 때문!



참조: https://github.com/axios/axios/issues/383