npm i -D @svgr/webpack

## NEXT

next는 next.config.js를 프로젝트 루트에 작성하여 기존 설정을 수정할 수 있게 함

```js
// `next.config.js` 작성하기
module.exports = {
  webpack(config) {
    config.module.rules.push({
      // 웹팩설정에 로더 추가함
      test: /\.svg$/,
      issuer: {
        test: /\.(js|ts)x?$/,
      },
      use: ["@svgr/webpack"],
    });

    return config;
  },
};
```

## REACT

```js
webpack.config.js;

module.exports = {
  // ...생략
  module: {
    rules: [
      //... 로더 생략
      {
        test: /\.svg$/,
        use: ["@svgr/webpack"],
      },
    ],
  },
  // ... 생략
};
```

```tsx
import Star from "./star.svg"; // 이미지를 불러오면 컴포넌트에 담김!

const App = () => (
  <div>
    <Star /> // 컴포넌트로 사용
  </div>
);
```

출처: <https://velog.io/@hwang-eunji/svg-%ED%8C%8C%EC%9D%BC-react-next%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0>
