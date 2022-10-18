useParams는 리액트에서 제공하는 Hook으로 동적으로 라우팅을 생성하기 위해 사용한다.

URL에 포함되어있는 Key, Value 형식의 객체를 반환해주는 역할을 한다. Route 부분에서 Key를 지정해주고, 해당하는 Key에 적합한 Value를 넣어 URL을 변경시키면, useParams를 통해 Key, Value 객체를 반환받아 확인할 수 있다. 반환받은 Value를 통해 게시글을 불러오거나, 검색목록을 변경시키는 등 다양한 기능으로 확장시켜 사용할 수 있다.

* url로 넘긴 변수값을 가져와 활용할 수 있는 훅: localhost/app/hi 이런식이면 url로 넘긴 hi라는 변수를 코드로 사용할 수 있는 것임


! 쿼리,param,바디에 대해 알고 있어야 위 글을 이해할 수 있음
참조: https://phsun102.tistory.com/m/92