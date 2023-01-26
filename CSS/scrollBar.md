## 내용이 넘쳐날때/스크롤이 필요할때: `overflow: auto`  
y축에만 적용  
* overflow-y: scroll;

![오버플로우](/images/overFlow.png)


다른 child 컴포넌트의 높이를 고려하여 overflow하는법 - 굳이 height안줘도 됨

부모 - display:flex, column으로 해주고 아래 자식 컴포넌트에 그냥 overflow:auto하면됨

## 스크롤바 디자인하기
* ::-webkit-scrollbar : 스크롤바 영역에 대한 설정
* ::-webkit-scrollbar-thumb : 스크롤바 막대에 대한 설정
* ::-webkit-scrollbar-track  : 스크롤바 뒷 배경에 대한 설정

```css
scrollBar는 단순 div영역이라고 생각
.scrollBar::-webkit-scrollbar {
    width: 10px;  /* 스크롤바의 너비 */
}

.scrollBar::-webkit-scrollbar-thumb {
    height: 30%; /* 스크롤바의 길이 */
    background: #217af4; /* 스크롤바의 색상 */
    
    border-radius: 10px;
}

.scrollBar::-webkit-scrollbar-track {
    background: rgba(33, 122, 244, .1);  /*스크롤바 뒷 배경 색상*/
}
```
### 스크롤 바 thumb 부분 높이 조절이 안되는 경우
- 스크롤을 적용한 영역의 스크롤 가능 길이가 짧아 높이가 조절이 안될 수 있음
- thumb의 길이를 조절하는 부분은 min-height가 아닌 max-height로 적용됨
- 현재 가능한 스크롤 영역바에서 더 작아지지는 X