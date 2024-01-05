# 타입스크립트 절대경로 설정(alias)

모듈 해석 경로가 잘못 설정된 경우: TypeScript는 기본적으로 상대 경로 또는 node_modules 내의 모듈만 해석할 수 있습니다. 'src'나 '~'와 같은 절대 경로를 사용하려면, tsconfig.json 파일에서 'baseUrl' 및 'paths' 옵션을 설정해야 합니다.

예를 들어, 다음과 같이 설정할 수 있습니다:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "src/*": ["src/*"],
      "~/*": ["src/*"]
    }
  }
}
```

이 설정은 'src'와 '~'를 프로젝트의 'src' 디렉토리로 해석하도록 지시합니다. 이렇게 하면 'src/pages/Home' 또는 '~/pages/Home'과 같은 경로를 사용할 수 있습니다.

https://velog.io/@ahndong2/Typescript-%EC%A0%88%EB%8C%80-%EA%B2%BD%EB%A1%9C-%EC%84%A4%EC%A0%95
https://www.inflearn.com/questions/778551/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-import-%EC%A0%88%EB%8C%80%EA%B2%BD%EB%A1%9C

## 추가 설정

위와 같이 설정 이후에 IDE(vscode)에서 해당 경로를 사용하여 import 경로를 설정하였을 때 error 표기는 보이지 않을 것이다.

import Button from '@/components/Button";
...

이후 브라우저에서 해당 환경을 실행할 경우에 다음과 같은 에러를 만나게 된다.

Module not found: Error: Can't resolve '@/components/Button'

이는 typescript 컴파일 시에 경로는 tsconfig를 활용하여 경로 추적이 가능하지만, 실제 webpack 번들에서는 해당 경로가 고려되지 않은 채 빌드되어 번들 파일이 생성되고, 브라우저에서 스크립트 실행 시 해당 경로를 추적하지 못하여 모듈을 찾지 못하여 에러가 발생하는 것이다.

tsconfig.json 파일에서 baseUrl과 paths 설정만 추가하고 이후 과정을 건너뛴다면, 컴파일러는 타입스크립트 코드를 컴파일할 때 설정된 절대경로를 인식하지 못해 모듈을 찾지 못하고 컴파일 에러가 발생한다.

cra로 셋팅한 프로젝트은 웹펙에서도 설정을 해줘야한다. eject를 하지 않고 웹팩 설정을 바꾸는 방법은 react-app-rewired를 사용하는 것이다.
참고: https://daisy-mansion.tistory.com/127
참고2 (이걸로 해결완료): https://kimsangyeon-github-io.vercel.app/blog/2022-05-09-cra-absolute-path

사용가능한 패키지: https://www.npmjs.com/package/react-app-rewire-alias // `yarn add --dev react-app-rewired`

https://velog.io/@dikum98/React-with.-CRA-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-Typescript-%EC%A0%81%EC%9A%A9%EC%8B%9C%ED%82%A4%EA%B8%B0-fdetxw7h
