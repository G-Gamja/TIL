# setTimeout 함수는
setTimeout() 사용법
어떤 코드를 바로 실행하지 않고 일정 시간 기다린 후 실행해야하는 경우가 있는데요. 이럴 때는 자바스크립트의 setTimeout() 함수를 사용할 수 있습니다.

setTimeout() 함수는 첫번째 인자로 실행할 코드를 담고 있는 함수를 받고, 두번째 인자로 지연 시간을 밀리초(ms) 단위로 받습니다.

간단한 예로, 2초를 기다린 후에 어떤 문자열을 콘솔에 출력해보겠습니다.

setTimeout(() => console.log("2초 후에 실행됨"), 2000);


참조: https://www.daleseo.com/js-timer/