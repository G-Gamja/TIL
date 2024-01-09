https://wikidocs.net/160308

이거를 추천함

https://jeonghwan-kim.github.io/js/2017/05/15/webpack.html

웹팩은 모듈 번들러이다. 모듈은 각 리소스 파일을 말하고, 번들은 웹팩 실행후에 나오는 결과 파일이다. 하나의 번들 파일은 여러 모듈로 만들어진다

## 왜 필요할까

과거에는 자바스크립트의 역할이 돔을 조작하는 간단한 역할만 수행했으면 됐지만 오늘날에 있어서 웹페이지를 구성할 때 수백개의 js파일이 필요하기 때문에 이를 모두 임포트 하기에는 무리이며 실행되는 순서도 신경써야하고 선언한 전역변수가 겹쳐 오버라이딩되는 이슈가 발생할 수 있기 때문에 여러 모듈을 쉽게 묶어줄 수 있는 웹펙이 필요해진것이다.

크롬 익스텐션에서는 다른 프레임워크와는 다르게 바로 모듈화를 할 수 없다는 점에서 그 고민은 더욱 커집니다.  
웹팩이란 JS를 위한 모듈 번들러로 하나의 Entry point로부터 의존성이 있는 모듈들을 묶어 하나의 파일로 만들어줍니다.// 자바스크립트로 만든 프로그램을 배포하기 좋은 형태로 묶어주는 툴이다.

- 자바스크립트의 모듈시스템이 commonJs이다.

리액트와 같은 대표적인 웹 프레임워크에서는 create-react-app과 같은 명령어를 쓰면 자동으로 웹팩이 설치되어 자동으로 모듈화를 할 수 있지만 Chrome Extension은 그렇지 않으므로 직접 설정해야 하는 부분이 조금 있습니다.

## 로더란

로더(loader)는 모듈을 입력으로 받아서 원하는 형태로 변환한 후 새로운 모듈을 출력해주는 함수이다

## 플러그인

로더는 특정 모듈에만 적용이 가능하지만 플러그인은 웹팩이 실행되는 전체 과정에 개입할 수 있다.

## 폴리필,polyfill

폴리필(Polyfill)은 최신 ECMAScript 환경을 만들어 준다. 바벨은 ES6 => ES5로 변환할 수 있는 것들만 변환을 하는데,

ES6에서 비동기 처리를 위해 등장한 Promise와 같이 ES5에서 변환할 수 있는 대상이 없는 경우 에러가 발생한다.

이러한 경우 우리는 Polyfill을 이용해서 이슈를 해결할 수 있다.

참조: https://iancoding.tistory.com/175
스택오버플로우: https://stackoverflow.com/questions/64557638/how-to-polyfill-node-core-modules-in-webpack-5

# !해결법

-> 그냥 안된다는 그 패키지를 설치해버리자

## INLINE_RUNTIME_CHUNK=false

INLINE_RUNTIME_CHUNK=false disabled webpack generating inline JavaScript in HTML. Normally webpack will put its own runtime into HTML inline script. But inline script is not allowed by browser extension.

## 사용하는 패키지에서 dependency로 걸려있는 패키지가

참조: https://github.com/microsoft/PowerBI-visuals-tools/issues/365
module: {
rules: [
{
test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
      {
        test: /\.css$/i,
use: ['style-loader', 'css-loader'],
},
{
test: /\.(png|jpe?g|gif)$/i,
        loader: 'file-loader',
      },
      {
        test: /\.svg$/,
use: ['@svgr/webpack'],
},
{
test: /\.m?js/,
resolve: {
fullySpecified: false,
},
},
],
},
