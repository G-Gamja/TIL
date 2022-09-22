# 이벤트란
웹페이지에서 마우스를 클릭했을 때, 키를 입력했을 때, 특정요소에 포커스가 이동되었을 때 어떤 사건을 발생시키는 것 
* ex. 이벤트 종류
    * 포커스 이벤트(focus, blur)
    * 폼 이벤트(reset, submit)
    * 뷰 이벤트(scroll, resize)
    * 키보드 이벤트(keydown, keyup)  
    * 마우스 이벤트(mouseenter, mouseover, click, dbclick, mouseleave)  
    * 드래그 앤 드롭 이벤트 (dragstart, drag, dragleave, drop)

## 이벤트 핸들러
* 사용자가 실제 이벤트를 발생시켰을 때 그 이벤트에 대응하여 처리하는 것  =>  `'이벤트 핸들러'`
* `'이벤트 핸들러'`는 앞에` 'on'`을 붙여 주고 이벤트에 대한 `동작(함수)을 처리 `

## 이벤트 리스너

`이벤트 리스너`는 DOM 객체에서 이벤트가 `발생`할 경우 해당 이벤트 처리 `핸들러를 추가`할 수 있는 `오브젝트이다`.
이벤트 리스너를 이용하면 특정 DOM에 위에 말한 Javascirpt` 이벤트가 발생할 때 특정 함수를 호출한다.`    

**해당 돔에 해당 이벤트 발생시 이벤트 핸들러를 작동시킴**

### 이벤트 리스너 등록
```typescript
DOM객체. addEventListener(이벤트명, 실행할 함수명, 옵션)

document.addEventListener('mousedown', handler);//handler라는 함수를 따로 생성
```
* 이벤트명 : Javascript에서 발생할 수 있는 이벤트 명을 입력한다.
* 함수명 : 해당 변수는 생략 가능하며, 해당 이벤트가 발생할 때 실행할 함수 명을 입력한다.
* 옵션: 옵션은 생략이 가능하며, 자식과 부모 요소에서 발생하는 버블링을 제어하기 위한 옵션이다.
### 이벤트 리스너 삭제
이벤트 리스너는 `자동으로 삭제되지 않기 때문에` `useEffect훅과` 함께 사용하여 cleanup함수에 제거 함수를 사용하도록 권장함
```typescript
`형태`

DOM객체. removeEventListener(이벤트명, 실행했던 함수명);

`예시`
useEffect(() => {
    // 이벤트 핸들러 함수
    const handler = (event: MouseEvent): void => {
      if (dialogRef.current && !dialogRef.current.contains(event.target as Node)) {
        onCancel();
      }
    };
    // 이벤트 핸들러 등록
    document.addEventListener('mousedown', handler);
    return () => {
      // 이벤트 핸들러 해제
      document.removeEventListener('mousedown', handler);
    };
  }, [dialogRef, onCancel]);
```




