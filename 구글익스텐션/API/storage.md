chrome storage 는 아래처럼 조금 다른점을 제외하고는 localStrage API 와 거의 동일한 기능을 제공

- 사용자 데이터는 storage.sync 를 사용하여 Chrome 과 자동으로 동기화 가능하다.
- 데이터는 객체로 저장할 수 있다. (localStorage API 는 데이터를 문자열로 저장한다.)
- 읽기 및 쓰기 작업을 비동기로 처리하여 빠르다.
- extension 콘텐츠 스크립트는 background page 없이도 사용자 데이터에 직접 접근할 수 있다.
- 시크릿 모드 incognito 사용중에도 user 의 extension settings 는 유지될 수 있다.
- 관리자가 'storage.managed' 로 설정한 데이터도 읽어올 수 있다.

https://velog.io/@devjuun_s/Chrome.storage-%EC%A0%95%EB%A6%AC
