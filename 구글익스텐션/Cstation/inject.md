노드 생성하기
1. Html tag 노드 생성하기 (createElement 메서드)

```javascript
 // document.createElement(tagName);
const divElement = document.createElement('div');
```
document문서의 createElement(tagName) 메소드를 활용하면, 태그 노드를 생성할 수 있다.

2. 노드 삽입하기(appendChild)
    
위와 같은 모양으로 appendChild메서드를 사용하면 노드 안에 노드를 삽입할 수 있다.

단, 메서드 이름에서도 알 수 있듯, 자식 노드로 삽입이 되기 때문에

추가할 노드의 부모 노드에 먼저 접근해야 한다.
```javascript
let newH1 = document.createElement('h1');
let newTitle = document.createTextNode('Hi!?');

newH1.appendChild(newTitle);
```
```html
<h1>Hi!?</h1>
```

위와 같은 노드가 담겨 있는 것이라고 볼 수 있다.

 

자, 그런데 아직 이 노드는 바로 웹페이지에 반영되는 것이 아니라, 가상의 공간에 생성되기만 한 것이다.

그래서 이미 존재하는 노드에 접근해서 생성한 노드를 한 번 더 삽입해 주어야 웹페이지에 반영되는 것이다.


컨테이너 태그에 집어넣기
```javascript
let container = document.getElementById('container');

let newH1 = document.createElement('h1');
let newTitle = document.createTextNode('Hi!?');

newH1.appendChild(newTitle);
container.appendChild(newH1);
```