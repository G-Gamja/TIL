# MetaMask - RPC Error: Request of type 'wallet_switchEthereumChain' already pending for origin http://www.localhost:3000. Please wait.

아마 익스텐션에 요청하는 훅을 useEffect로 처리해놨는데 요청을 중복으로 걸려있어서 이런 오류가 있는것으로보임

깃헙 이슈: https://github.com/MetaMask/metamask-extension/issues/10584
