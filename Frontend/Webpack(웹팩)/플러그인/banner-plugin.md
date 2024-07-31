# 간편한 배포 확인: banner-plugin

이 제품은 정기적으로 작업물을 배포합니다. 개발자는 잘 배포되었는지 확인하고 관련 채널에 알리는데요. 저는 젠킨스에서 배포 완료를 확인한 뒤 배포된 제품도 바로 확인합니다. 직접 눈으로 보아야 배포가 잘 되었다는 걸 확신할 수 있기 때문입니다.

문구 수정 같은 간단한 기능은 금방 확인할 수 있습니다만, 특정한 조건에서 발생하는 작업은 확인하는데 좀 까다롭습니다. 그만큼 배포 소식도 늦어져 애를 태우기도 합니다.

좀 더 직관적으로 배포 여부를 확인하는 방법이 있을까요? 프론트엔드 코드를 배포하는 것은 결국 서버 어딘가에 정적파일을 업로드하는 것인데요. 이 정적파일에 배포일자를 기록해 두면 좋겠습니다. 이 정보만 확인하면 쉽게 배포 여부를 알 수 있을 것 같습니다.

웹팩 기본 플러그인 중 banner-plugin은 빌드 결과물에 주석을 추가하는 녀석입니다. 빌드 날짜를 주석으로 기록해 둔다면 배포 여부를 이것으로 갈음할 수 있겠습니다.

```js
// webpack.config.js
// `webpack` command will pick up this config setup by default
var path = require("path");
const Webpack = require("webpack");

const banner = `빌드 날짜: ${new Date().toLocaleString()}`;

module.exports = {
  mode: "none",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist"),
  },
  plugins: [new Webpack.BannerPlugin({ banner })],
};
```

![리다이렉트](/images/banner-plugin-ex.png)

만약 배너가 보이지 않는다면 웹팩의 TerserPlugin 옵션을 확인해 보세요. 이 플러그인은 코드를 압축하는데 이 과정에서 배너를 삭제할 수도 있기 때문입니다.

```js
// webpack.config.js

const TerserPlugin = require('terser-plugin');

optimization: {
  minimizer: [
    // 기본 플러그인을 커스터마이징
    new TerserPlugin({
      terserOptions: {
        output: {
          // 주석을 제거한다.
          comments: false,
          // 아래 주석은 유지한다.
          preamble: /* ${banner} */,

```

preamble(서두, 서문) 옵션에 압축 대상에서 제외할 문구를 지정할 수 있습니다. 여기에 배너 문구를 추가해두면 플러그인은 이를 건너뛰고 코드를 압축합니다. 결과물에 배너를 유지할 수 있겠죠.

이제 배포하고 나면 직접 기능을 확인하기 전에 정적 파일을 열어 봅니다. 코드 상단에 빌드 정보를 확인하고 배포여부를 알 수 있기 때문입니다.

출처: https://techblog.woowahan.com/6465/
