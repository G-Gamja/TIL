# tsconfig란

tsconfig 파일 생성하기

여러분 프로젝트 폴더에 tsconfig.json 이라는 파일을 하나 생성합시다.

여기엔 타입스크립트 ts 파일들을 .js 파일로 변환할 때 어떻게 변환할 것인지 세부설정이 가능합니다.

리액트나 뷰 이런거 쓰는 중이면 이미 있을 수 있음

▲ 이렇게 그냥 자신있게 만들면 됩니다

{
"compilerOptions": {
"target": "es5",
"module": "commonjs",
}
}
그리고 json파일 안에 이렇게 복붙하도록 합시다.

'target'은 타입스크립트파일을 어떤 버전의 자바스크립트로 바꿔줄지 정하는 부분입니다.

es5로 셋팅해놓으면 es5 버전 자바스크립트로 컴파일(변환) 해줍니다.

신버전을 원하면 es2016, esnext 이런 것도 입력할 수 있습니다.

'module'은 자바스크립트 파일간 import 문법을 구현할 때 어떤 문법을 쓸지 정하는 곳입니다.

commonjs는 require 문법

es2015, esnext는 import 문법을 사용합니다.

그래서 어느정도 IE 호환성을 원하시면 es5, commonjs가 국룰임

근데 정말 신버전 자바스크립트만 표현가능한 그런 문법들이 있는데

(예를 들어 BigInt() 이런 함수와 bigint 타입)

그런 것들은 esnext 등으로 버전을 올려줘야 사용가능합니다.

추가로 넣을만한 것들

{
"compilerOptions": {
"target": "es5",
"module": "commonjs",
"noImplicitAny": true,
"strictNullChecks": true
}
}
noImplicitAny는 any라는 타입이 의도치않게 발생할 경우 에러를 띄워주는 설정이고

strictNullChecks는 null, undefined 타입에 이상한 조작하면 에러를 띄우는 설정입니다.

저는 이것들은 안넣고 시작할겁니다.

나중에 실제 개발할 때 넣어보셈

tsconfig에 들어갈 기타 항목들

대부분 건드릴 필요 없는데 쓸모있어보이는 것들만 추려봤습니다.

전체는 https://www.typescriptlang.org/tsconfig 에서 구경가능합니다.

```json
{
  "compilerOptions": {
    "target": "es5", // 'es3', 'es5', 'es2015', 'es2016', 'es2017','es2018', 'esnext' 가능
    // NOTE 아웃풋이 es5형태의 자바스크립트로 나옴
    "module": "commonjs", //무슨 import 문법 쓸건지 'commonjs', 'amd', 'es2015', 'esnext'
    // NOTE 모듈을 어떻게 컴파일할지에 대한 설정입니다.
    // import와 export 구문을 사용하여 모듈을 정의하고 가져오는 방식을 의미합니다.
    // NOTE 아래의 코드를 이제 사용 가능

    // TypeScript 코드
    // export const foo = 42;
    // import { foo } from './moduleA';

    "moduleResolution": "node",
    // NOTE import쪽에서 모듈을 가져올때 그 가져오는 방식을 노드쪽을 따라한다는 의미
    //     예를 들어, 다음과 같은 코드가 있을 때:

    // TypeScript는 다음과 같은 순서로 모듈을 찾습니다:
    // import * as fs from 'fs';
    // import myModule from './myModule';
    //1. fs 모듈은 node_modules 폴더에서 찾습니다.
    //2. ./myModule 모듈은 현재 디렉토리에서 myModule.ts, myModule.tsx, myModule.js, myModule.json 등의 파일을 순서대로 찾습니다.

    "allowJs": true, // js 파일들 ts에서 import해서 쓸 수 있는지
    "checkJs": true, // 일반 js 파일에서도 에러체크 여부
    "jsx": "preserve", // tsx 파일을 jsx로 어떻게 컴파일할 것인지 'preserve', 'react-native', 'react'
    "declaration": true, //컴파일시 .d.ts 파일도 자동으로 함께생성 (현재쓰는 모든 타입이 정의된 파일)
    "outFile": "./", //모든 ts파일을 js파일 하나로 컴파일해줌 (module이 none, amd, system일 때만 가능)
    "outDir": "./", //js파일 아웃풋 경로바꾸기
    "rootDir": "./", //루트경로 바꾸기 (js 파일 아웃풋 경로에 영향줌)
    "removeComments": true, //컴파일시 주석제거

    "strict": true, //strict 관련, noimplicit 어쩌구 관련 모드 전부 켜기
    "noImplicitAny": true, //any타입 금지 여부
    "strictNullChecks": true, //null, undefined 타입에 이상한 짓 할시 에러내기
    "strictFunctionTypes": true, //함수파라미터 타입체크 강하게
    "strictPropertyInitialization": true, //class constructor 작성시 타입체크 강하게
    "noImplicitThis": true, //this 키워드가 any 타입일 경우 에러내기
    "alwaysStrict": true, //자바스크립트 "use strict" 모드 켜기

    "noUnusedLocals": true, //쓰지않는 지역변수 있으면 에러내기
    "noUnusedParameters": true, //쓰지않는 파라미터 있으면 에러내기
    "noImplicitReturns": true, //함수에서 return 빼먹으면 에러내기
    "noFallthroughCasesInSwitch": true //switch문 이상하면 에러내기
  }
}
```
