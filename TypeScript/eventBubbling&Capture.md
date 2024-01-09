https://choonse.com/2022/01/14/651/

공식 독스: https://ko.javascript.info/bubbling-and-capturing

# 이벤트 버블링이란

이벤트 버블링은 이벤트 발생 요소부터 시작해 최상위 부모요소(document)까지 이벤트가 연달아 발생하는 현상을 뜻한다.

이벤트 버블링은 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미합니다.

```
상위의 화면 요소란? HTML 요소는 기본적으로 트리 구조를 갖습니다. 여기서는 `트리 구조상으로 한 단계 위에 있는 요소를 상위 요소`라고 하며 body 태그를 최상위 요소라고 부르겠습니다.
```

![이벤트버블링](/images/event-bubble.png)

    <style>
    .upper{
        background:gold;
        width:70px;
        text-align:center;
    }
    .middle{
        background:orange;
        width:50px;
        text-align:center;
    }
    .lower{
        background:pink;
        width:30px;
    }
    </style>

    <script>
    function clicked(event){
        alert(event.currentTarget.getAttribute('id'));
    }
    </script>

    <div class="upper" onClick="clicked(event)" id="쌉">a
    <div class="middle" onClick="clicked(event)" id="가">b
        <div class="lower" onClick="clicked(event)" id="능">c
        </div>
    </div>

    </div>

코드의 결과는 아래와 같다

<style>
  .upper{
    background:gold;
    width:70px;
    text-align:center;
  } 
  .middle{
    background:orange;
    width:50px;
    text-align:center;
  }
  .lower{
    background:pink;
    width:30px;
  }
</style>

<script>
  function clicked(event){
    alert(event.currentTarget.getAttribute('id'));
  }
</script>

<div class="upper" onClick="clicked(event)" id="쌉">a
  <div class="middle" onClick="clicked(event)" id="가">b
    <div class="lower" onClick="clicked(event)" id="능">c
    </div>
  </div>
  
</div>  
 
여기서 가장 내부요소인 c(lower/id=능)을 클릭하면 어떤 순서대로 이벤트가 실행될까

- 이벤트 버블링이 default로 설정되어 있으므로 c,b,a 순서대로 이벤트가 발생한다  
  결과: `능->가->쌉` //작은거-> 큰거로 연달아서..

b 클릭 시 `가-> 쌉 `으로 이벤트가 발생 됨

브라우저는 특정 화면 요소에서 이벤트가 발생했을 때 그 이벤트를 최상위에 있는 화면 요소까지 이벤트를 전파시킵니다. 따라서, 클래스 명 three -> two -> one 순서로 div 태그에 등록된 이벤트들이 실행됩니다. 마찬가지로 two 클래스를 갖는 두 번째 태그를 클릭했다면 two -> one 순으로 클릭 이벤트가 동작하겠죠.

여기서 주의해야 할 점은 `각 태그마다 이벤트가 등록`되어 있기 때문에 `상위 요소로 이벤트가 전달`되는 것을 확인할 수 있습니다. 만약 이벤트가 `특정 div 태그에만 달려 있다면 `위와 같은 동작 결과는 **확인할 수 없습니다.**

# 이벤트 캡쳐

순서를 `‘쌉’ -> ‘가’ -> ‘능’ (부모->자식)`으로 만들려면 어떻게 해야 할까요? 바로 이벤트 캡쳐를 사용하면 됩니다.  
![이벤트버블링](/images/event-bubble.png)
이벤트 캡쳐는 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식입니다.

이벤트 캡쳐를 사용하려면 addEventListener 내부에 capture 값을 명시적으로 true로 변경해줘야 합니다.

기본값은 `false`이며, `false`는 `이벤트 버블링,` `true`는 `이벤트 캡쳐`를 의미합니다.

```JSX
function clicked(event){
        alert(event.currentTarget.getAttribute('id'));
    }

    const divs = document.querySelectorAll('div');
    divs.forEach(function(div) {
        div.addEventListener('click', clicked,{
          // 이벤트 리스너의 capture 속성값을 바꾼 모습
          capture:true
      }
      );
    });

    <div class="upper" id="쌉">a
      <div class="middle" id="가">b
        <div class="lower" id="능">c
        </div>
      </div>
    </div>
```

위 코드에서 capture:true로 설정해주고 다시 c를 클릭하면 이번에는 반대 순서인 위에서 아래로 이벤트가 발생합니다.

이제는 ‘쌉’ -> ‘가’ -> ‘능’ 의 순서로 알림이 뜹니다.

그렇다면 ‘이벤트 버블링도 싫고, 이벤트 캡쳐도 싫으니 둘 다 하지마!’라고 명령할 때는 어떻게 해야 할까요?

# event.stopPropagation()

이벤트의 증식(버블링)을 멈추기 위해 사용한다  
이벤트 핸들러 함수에 함수(스탑)를 넣어준다

```JSX
이벤트 핸들러 함수
function clicked(event){
  // 여기에 스탑프로파게이션 함수를 넣어줌
   event.stopPropagation();
        alert(event.currentTarget.getAttribute('id'));
    }

<div class="upper" onClick="clicked(event)" id="쌉">a
  <div class="middle" onClick="clicked(event)" id="가">b
    <div class="lower" onClick="clicked(event)" id="능">c
    </div>
  </div>
```

이렇게 되면 쌉->가->능 도 아닌 쌉, 가 이렇게 `해당 요소의 이벤트만` 실행한다

# 이벤트 위임 - Event Delegation

‘하위 요소에 각각 이벤트를 붙이지 않고 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식’입니다.

```html
<h1>오늘의 할 일</h1>
<ul class="itemList">
  <li>
    <input type="checkbox" id="item1" />
    <label for="item1">이벤트 버블링 학습</label>
  </li>
  <li>
    <input type="checkbox" id="item2" />
    <label for="item2">이벤트 캡쳐 학습</label>
  </li>
</ul>
```

```javascript
var inputs = document.querySelectorAll("input");
inputs.forEach(function (input) {
  input.addEventListener("click", function (event) {
    alert("clicked");
  });
});
```

자바스크립트 querySelectorAll()를 이용해 화면에 존재하는 모든 인풋 박스 요소를 가져온 다음 각 인풋 박스의 요소에 클릭 이벤트 리스너를 추가합니다. 화면을 실행시키고 각 리스트 아이템의 인풋 박스(체크 박스)를 클릭하면 위와 같이 경고 창이 표시되죠.

여기까지는 별다를 것 없는 이상하지 않은 코드였는데요. 그런데 만약 여기서 할 일이 더 생겨서 리스트 아이템을 추가하면 어떻게 될까요?

추가한 아이템에는 이벤트 리스너가 달리지 않아 아무 이벤트도 발생하지 않는다
=> 아이템 각각에 이벤트 리스너를 붙이는 것보다 그 아이템들을 감싸는 더 상위 요소에 이벤트 리스너를 붙이면 된다

# event.target vs event.currentTarget

event.target은 실제 이벤트가 발생한 요소를 반환합니다.
event.currentTarget은 이벤트가 걸려있는 요소를 반환합니다.
