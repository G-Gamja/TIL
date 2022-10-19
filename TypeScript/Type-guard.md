# Type Guard

타입스크립트가 어떤 변수에 대해 타입을 확실히 할 수 있게끔 narrowing해주는 것
=> 이를 통해 해당 변수의 로컬필드에 접근할 수 있다
(e.g type을 isAxiosError메서드로 검증 한 후라면 검증한 변수가 엑시오스 에러 객체임을 확신할 수 있고 엑시오스 에러의 로킬 변수를 참조할 수 있게된다.

참조: https://ibocon.tistory.com/m/261
참조2: https://ibocon.tistory.com/m/261