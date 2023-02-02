# 디바운스와 스로틀링
- 간단설명
    - 쓰로틀링: 마지막 함수가 호출된 후 일정 시간이 지나기 전에 다시 호출되지 않도록 하는 것
    - 디바운싱: 연이어 호출되는 함수들 중 마지막 함수(또는 제일 처음)만 호출하도록 하는 것

# 디바운스  

디바운스 -초 뒤의 콜 하나만

인풋에 대해 어떤 함수가 실행되고 어떤 시간안에 그 함수가 다시 들어오면 다시 딜레이만큼 기달리고 그 함수 호출이 끝나고 설정한 딜레이 만큼 기다렸다가 또 호출이 없으면 호출이 된다.(주기를 가지지 않고)

딜레이를 2초를 했다면
함수 실행 후 2초 동안 다시 그 함수가 호출된다면? -> 다시 2초 기다리고 -> 2초 사이에 함수가 호출이 안된다면? 그때 실행



옵션에 따라 첫함수는 시작하고 나서 딜레이만큼 기달렸다가 또 호출을 받을수도있고(입력 맨 처음 , 입력 다하고 딜레이까지 기다린 후 호출) 아니면 딜레이 걸어놓고 입력 끝난 후 딜레이 끝난 후 호출(이러면 호출 이 맨 마지막에만 호출이 됨)

→ onChange에서 보통 사용됨

# 스로틀링  
스로틀은 주기성을 가지고 무조건 딜레이만큼 끊어서 그 딜레이 

스로틀링 아묻따 딜레이로 설정한 시간 안에서는 아무리 호출해도 한번만 실행되는거임 -> 함수 호출이 확정적임   
딜레이건 시간마다 한번씩만 호출

→ 보통은 스크롤에서 많이 씀 → 디바운스는 예를 들어서 마지막 맨 아래에 도착하고 나서야 딜레이기간을 기다리기때문에 오래 기달려야하는데 스로틀은 어느 구간을 지나면 새로 get할 수 있기 때문에

→ 근데 스크롤을 맨 마지막으로 당겼을때만 기다려되는 상황에서는 디바운스를 적용시킬 수 있음 무조건 스크롤에 스로틀을 걸어야된다느게 아님

보통 onchange에서 사용함 - 최적화