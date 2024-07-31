# loader

왜 씀?

타입스크립트 같은 다른 언어를 자바스크립트 문법으로 변환해 주거나,
css나 img같은 파일을 자바스크립트에서 바로 import해서 사용할 수 있게 해주기 때문.

- css, img를 자바스크립트 코드로 변환한다고 생각하면 됨.

빌드 전

```css
body {
  background-color: green;
}
```

빌드 후

![리다이렉트](/images/css-loader.png)
출처: https://velog.io/@cjy0029/%EC%9B%B9%ED%8C%A9-%EB%A1%9C%EB%8D%94

! 모듈로 변경된 스타일시트는 돔에 추가되어야만 브라우저가 해석할 수 있다.

css-loader로 처리하면 자바스크립트 코드로만 변경되었을 뿐 돔에 적용한 것은 아니기 때문에 아직 적용이 안된 것 처럼 보인다

$ npm install -D style-loader  
그리고 웹팩 config에 로더를 추가한다.

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"], // style-loader를 앞에 추가한다
      },
    ],
  },
};
```

## file-loader

css에서 url() 함수에 이미지 파일 경로를 지정할 수 있는데 웹팩은 파일로더를 이용해서 이 파일을 처리한다

```css
body {
  background-image: url("./image.png");
}
```

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif)$/i,
        loader: "file-loader",
      },
    ],
  },
};
```

이렇게만 하면 이미지가 안나올 수 있는데
CSS를 로딩하면 background-image:url(bg.png) 코드에 의해 동일 폴더에서 이미지를 찾으려고 시도할 것이다. 그러나 웹팩으로 빌드한 이미지 파일은 output인 dist폴더 아래로 이동했기 때문에 이미지 로딩에 실패한 것이다

file-loader 옵션을 조정해주자.

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.png$/, // .png 확장자로 마치는 모든 파일
        loader: "file-loader",
        options: {
          publicPath: "./dist/", // prefix를 아웃풋 경로로 지정
          name: "[name].[ext]?[hash]", // 파일명 형식
        },
      },
    ],
  },
};
```

출처: https://velog.io/@cjy0029/%EC%9B%B9%ED%8C%A9-%EB%A1%9C%EB%8D%94

## ts-loader

만약 타입스크립트, 웹팩을 사용하는 환경이라면

```bash
tsc & webpack
```

이렇게 트랜스 파일 후 빌드를 하게 될텐데 이러면 프로세스가 두개가 돌아가게 되어 빌드 시간이 오래 걸린다.
타입스크립트 컴파일 단계를 웹팩 안으로 통합할 수 있기 때문이죠. 웹팩 로더 중 ts-loader를 추가하면 웹팩만 실행하더라도 두 가지 일을 동시에 처리할 수 있습니다.

```js
// webpack.config.js

module: {
  rules: [{
    test: /.tsx?$/,
    // tsc를 직접 실행하지 않고 로더를 추가
    loader: 'ts-loader'
```

번들링 속도 개선 (fork-ts-checker-webpack-plugin)

fork-ts-checker-webpack-plugin 은 타입스크립트 타입 검사를 별도로 실행하는 웹팩 플러그인입니다. 웹팩을 실행하면 먼저 컴파일과 번들링만 빨리 실행하고 타입 체크는 좀 늦게 따로 실행할 수 있습니다.

```js
// webpack.config.js

const ForkTsCheckerWebpackPlugin = require('fork-ts-checker-webpack-plugin');

plugins: [
  // 플러그인을 추가
  new ForkTsCheckerWebpackPlugin()
]

module: {
  rules: [{
    test: /.tsx?$/,
    loader: 'ts-loader',
      options: {
        // 타입체크는 fork-ts-checker-webpack-plugin이 수행
        transpileOnly: true,
     },
  }],
},
```
