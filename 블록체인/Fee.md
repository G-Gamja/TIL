# FEE가 가지는 의의
This fee prevents spamming the network with useless transactions.

가스 * 가스 레이트(tiny,low,average) = amount
e.g) 150000 * 0.0025 = 375

여기에 해당 fee코인의 decimals를 적용시켜주면 
display용 fee가 됩니다

tx의 fee필드에는
fee = {
    amount = {
        amount: -
        denom:
    }
    gas : 
}   
를 정의해줘야함

COSMOS-SDK 백서: https://docs.cosmos.network/main/basics/gas-fees

원론적으로 tx에 fee필드에 amount가 리스트타입으로 받는 이유가 복수의 코인을 합쳐서 수수
료 계산에 사용할 수 있도록 하기 위함이다.

하지만 우리는 그렇게 까지는 하지 않을것이며 스테이킹 외 다른 토큰 코인도 수수료 지불
수단으로 지원하기만 할 것이다(단일 토큰).