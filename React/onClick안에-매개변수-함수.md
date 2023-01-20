온클릭에 메서드를 넘겨줄때 이 메서드에서 파라미터가 정의되어있다면
onclick{method(argument)}이런 식으로 작성할건데 이러면 컴파일러가 method(argument)를 바로 실행시켜 버려서   
메서드 실행시점이 개발자가 원하는 시점과 상이해짐   
=> 그럼 어떡함? => 익명함수로 한꺼풀 감싸라 
사실 굳이 익명함수를 고집안해도 되긴한데 어차피 그 온클릭 스코프 안에서만 호출되니까 고되게 함수이름을 정의하면서 까지 할 필요는 없으니깐 ㅋㅎㅋ

```
onClick={() => {
          handleClick("donglee");
        }}
```
자세히 보면 익명함수 자체를 넘긴거고 호출이 아니기 때문에(`()`을 말함)     
클릭-> 넘겨진 익명함수를 그제서야 `호출`-> 익명함수 안 정의된 구문이 실행됨


### 온클릭 작동 프로세스
클릭시 btn.onclick 에 할당한 `함수`를 실행해요

btn.onclick = correct;
로 사용하시면 잘 동작할 거에요

btn.onclick = correct();
처럼 사용하면 브라우저가 코드를 해석하는 도중에 correct() 가 실행되고

함수 안에 alert 이 있어서 브라우저가 뜨자마자 알림창이 뜨게됩니다.

https://velog.io/@dom_hxrdy/reactJSX%EC%97%90%EC%84%9C-onClick%EC%9C%BC%EB%A1%9C-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98-%EB%84%A3%EC%96%B4%EC%A3%BC%EA%B8%B0