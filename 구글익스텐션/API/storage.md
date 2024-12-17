chrome storage 는 아래처럼 조금 다른점을 제외하고는 localStrage API 와 거의 동일한 기능을 제공

- 사용자 데이터는 storage.sync 를 사용하여 Chrome 과 자동으로 동기화 가능하다.
- 데이터는 객체로 저장할 수 있다. (localStorage API 는 데이터를 문자열로 저장한다.)
- 읽기 및 쓰기 작업을 비동기로 처리하여 빠르다.
- extension 콘텐츠 스크립트는 background page 없이도 사용자 데이터에 직접 접근할 수 있다.
- 시크릿 모드 incognito 사용중에도 user 의 extension settings 는 유지될 수 있다.
- 관리자가 'storage.managed' 로 설정한 데이터도 읽어올 수 있다.

https://velog.io/@devjuun_s/Chrome.storage-%EC%A0%95%EB%A6%AC

chrome.storage.sync.set 과 chrome.storage.set의 차이점

Chrome 확장 프로그램 개발에서 데이터를 저장하기 위해 자주 사용하는 chrome.storage API는 크게 sync와 local 두 가지 종류의 저장소를 제공합니다. 이 중에서 chrome.storage.sync.set과 chrome.storage.set은 각각 다른 저장소에 데이터를 저장하는 메서드입니다.

1. chrome.storage.sync.set
   구글 계정과 동기화되는 저장소: 사용자가 로그인한 모든 Chrome 브라우저에서 데이터를 동기화하여 공유합니다.
   용량 제한: 100KB까지 저장 가능하며, 개별 항목당 8KB, 최대 512개 항목까지 저장할 수 있습니다.
   사용 시나리오:
   사용자 설정을 여러 기기에서 동일하게 유지해야 할 때
   로그인 정보나 북마크처럼 사용자 계정과 연동되어야 할 데이터를 저장할 때
2. chrome.storage.set
   **chrome.storage.local.set의 줄임말:**local` 저장소에 데이터를 저장하는 메서드입니다.
   로컬 브라우저에만 저장되는 저장소: 해당 브라우저에서만 데이터에 접근할 수 있습니다.
   용량 제한: 일반적으로 sync 저장소보다 더 큰 용량을 사용할 수 있으며, 브라우저마다 다를 수 있습니다.
   사용 시나리오:
   사용자 설정을 로컬 브라우저에만 저장하고 싶을 때
   임시 데이터나 로그 데이터를 저장할 때
