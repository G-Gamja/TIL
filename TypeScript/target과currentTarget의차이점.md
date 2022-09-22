## 1. target과 currentTarget
우리는 이벤트를 다룰 때 target과 currentTarget의 프로퍼티를 사용해 해당 요소에 접근이 가능하다

ex. input창에서 입력 받기
```typescript
const onChangeMethod = (event: React.ChangeEvent<HTMLInputElement>) => {
    setEditName(event.currentTarget.value);
  };
```
그럼 이 둘의 차이점은 무엇인가
* target: 이벤트가 발생한 바로 그 요소를 직접 가리킴
* currentTarget: 이벤트 리스너를 가진 요소를 가리킴

예시 코드
```JSX
<style>
  .upper{
    background:gold;
    width:80px;
    text-align:center;
  } 
  .lower{
    background:pink;
    width:50px;
  }
</style>

<script>
  function clicked(event){
    alert(event.target.id+" clicked");
  }
</script>

<div class="upper" onClick="clicked(event)" id="div">
  <span class="lower" id="span">                  
    span
  </span>
</div>
```
위 코드는 아래와 같은 결과를 도출한다
<style>
  .upper{
    background:gold;
    width:80px;
    text-align:center;
  } 
  .lower{
    background:pink;
    width:50px;
  }
</style>

<script>
  function clicked(event){
    alert(event.target.id+" clicked");
  }
</script>

<div class="upper" onClick="clicked(event)" id="div">
  <span class="lower" id="span">                  
    span
  </span>
</div>

여기서 노랑은 div, 핑크는 span이며 핑크가 노란색 위에 있는 것이다.

이때 onClick이벤트는 div(노랑)에서 설정되었지만
span(핑크)를 클릭해도 이벤트가 발생한다

이는 이벤트 버블링, 이벤트 캡쳐와 관련이 있다...  https://choonse.com/2022/01/14/651/  

이벤트 발생에 따른 target은 다음과 같다

* `span(핑크)`를 클릭 시 나오는 alert문구
    * `target일` 경우: 핑크(핑크를 클릭했으니 핑크가 이벤트 발생 시점)
    * `currentTarget일` 경우: 노랑(onClick이벤트는 노랑(div)가 가지고 있음)
* `div(노랑)`를 클릭 시 나오는 alert문구
    * `target일` 경우: 노랑(노랑를 클릭했으니 노랑(div) 이벤트 발생 시점)
    * `currentTarget일` 경우: 노랑(onClick이벤트는 노랑(div)가 가지고 있음)

만약 핑크를 눌러도 이벤트를 가진 노랑의 속성에 접근하고 싶다면 currentTarget과 getAttribute를 사용하면 됩니다.

    <style>
    .upper{
        background:gold;
        width:80px;
        text-align:center;
    } 
    .lower{
        background:pink;
        width:50px;
    }
    </style>
    <script>
    function clicked(event){
        alert(event.currentTarget.getAttribute('id')+" clicked");
    }
    </script>
    <div class="upper" onClick="clicked(event)" id="div">
    <span class="lower" id="span">                  
        span
    </span>
    </div>
`target은` **누른 바로 그 것(누른 그 놈)**, `currentTarget은` 이벤트를 **실행하는 바로 그 것(onClick달려 있는 놈)**으로 이해하면 된다.