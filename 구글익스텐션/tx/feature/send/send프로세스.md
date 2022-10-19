# Extension Send Logic Process

1. currentAddress에 수신인 주소 입력
2.현재 선택한 체인에서 보낼 코인(체인의 네이티브코인),토큰을 선택
    * 선택에 따라 currentCoinOrToken가 변경
3.메모 텍스트 입력 -> currentMemo에 입력값 할당
4. errorMessage의 유뮤에 따라 하단의 send버튼의 활성화 여부를 결정짓는다(이때 !!errorMessage)를 통해 강제로 불리언형변환을 시켜주었다.
5. 큐스택에 cos_signAmino 메서드의 리퀘스트메시지를 enqueue한다.
6. 최상위의 Wrapper가 큐의 변화를 감지(useEffect훅)하고 큐에 갓 들어온 currentqueue의 메세지.메서드 명에 따라 정해진 패스로 라우팅을 해준다.
    * 그니께 enqueue하면 해당 메서드에 따라 정해진 페이지로 이동시켜준다 라고 진행과정을 이해하면 될듯


## 변수설명
vesting...: delegate만 가능하고 send는 불가능한 코인
coinList: {nativeStakingCoin , nativeCoin, ibcCoin}
originBaseDenom: ibc토큰의 경우 에러방지를 위해 baseDenom이 ibc/blahblah로 저장이 되기 때문에 원래의 baseDenom값을 따로 빼서 넣어둔것

## 궁금한 점
account.data?.value.sequence은 무엇을 의미하는가
