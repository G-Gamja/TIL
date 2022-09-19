## Callback 함수란


코드를 통해 명시적으로 호출하는 함수가 아니라, 개발자는 단지 함수를 동록하기만 하고, 어떤 이벤트가 발생했거나 특정 시점에 도달했을 때 시스템에서 호출하는 함수를 말한다.  

콜백함수라는 유니크한 문법적 특징을 가지고 있는 것이 아니라, 호출방식에 의한 구분이다. 
간단히 이야기 하면 어떤 함수가 실행을 끝낸 뒤 실행되는(우리는 이를 callback이라 부른다)함수를 말한다

1. 다른 함수의 인자로써 이용되는 함수.
2. 어떤 이벤트에 의해 호출되어지는 함수.

타입스크립트,자바스크립트 에서는 함수를 오브젝트로 간주하기 때문에 다른 오브젝트와 마찬가지로 다른 함수의 argument로 넘길 수도 return될 수도 있다.
이때 파라미터로 넘겨지게 되는 함수를 callback함수라 말한다  
`=>함수를 등록만 해놓고 어떤 이벤트가 발생했을 시 & 어느 시점이 되었을 때 시스템에서 호출하는 함수` 


콜백함수의 기본형태 
함수 func를 호출 시 param으로 받은 콜백함수가 실행된다.
`=> 따라서 제어권(콜백함수의 실행시점)이 func에게 있다`
```
function func(callback) {
	callback();
}
function callback() {
	console.log("callback이다");
}

func(callback);

결과 : callback이다
```
```
function introduce (lastName, firstName, callback) { 
	var fullName = lastName + firstName; 
	callback(fullName); 
} 

introduce("홍", "길동", function(name) { 
	console.log(name); 
    };

// 결과 -> 홍길동

출처: https://inpa.tistory.com/entry/JS-📚-자바스크립트-콜백-함수 [👨‍💻 Dev Scroll]
```
위 예제를 보면, `introduce` 함수를 실행하면, `callback자리를` 새로운 함수 `function(name)`으로 지정 해주면서 함수 안에서 `callback(fullname)`으로 실행 되는 함수가 된다.

## 콜백함수가 필요한 이유

* `비동기` 방식으로 작성된 함수를 `동기` 처리하기 위해 사용된다  
    * async, await등의 비동기 함수를 여타 다른 함수(동기)처럼 사용한다는 의미
* 동기: 하나의 요청이 오면 완료가 된 후 다음 요청을 실행하는 방식 - 흔한 순차적 로직흐름
* 비동기: 어떤 요청이 오면 완료가 되기 전에 다음 요청을 실행하는 방식
    *   하나를 실행시킨 후 응답이 올떄까지 모든 프로세스가 멈추기 때문에 응답이 오기전까지 안기다리고 그 다음꺼를 미리 해버리는 식
 
 ## 사용 예: 이벤트 핸들러
