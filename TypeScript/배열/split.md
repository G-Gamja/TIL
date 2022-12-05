# 배열 파싱하기

https://codechacha.com/ko/javascript-how-to-substring/

# 정규표현식을 사용하여 split하기 
*  split 아규먼트로 / / 안에 넣어주면 이를 정규식으로 인식한다.    
    - .split(/\s+/);

해석 예시
- 소수점 / 스트링 split하기
`const g = a.split(/([0-9]*[.]?[0-9]+)([a-zA-Z]*)/).filter((item)=> item.length > 0);`
우선 // 안에 값이 정규식임을 알리는 선언이랄까

([0-9]*[.]?[0-9]+): 이렇게 쌓여있으면 요거 하나를 청크로 보는거임

[0-9]: 0부터 9의 값의 문자셋을 의미
    . 혹은 *같이 정규식내에서 특별한 의미를 가지는 값이어도
    이 안에 들어가면 단순 캐릭터처리를 해버린다
    => 0 ~ 9 까지 의 수
* : 앞의 표현식이 0회 이상 연속으로 반복되는 부분과 대응
    [0-9]가 0회이상 있어야함
[.]: [] 안의 특수문자는 단순 문자처리를 하니 그냥 .을 의미  
?: 앞의 표현식이 0 혹은 1회이상 나와야함
+: 앞의 표현식이 1회이상 연속으로 반복되는 부분과 대응 즉 [0-9]가 여러번 나와야 함

가장 참고가 잘된 참고자료
https://codingpractices.tistory.com/entry/%EC%A0%95%EA%B7%9C-%ED%91%9C%ED%98%84%EC%8B%9D-%EC%A0%95%EA%B7%9C%EC%8B%9D-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0-RegExp

https://velog.io/@citron03/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-split-%EC%82%AC%EC%9A%A9-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D