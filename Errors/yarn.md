문제상황

cra한 후 yarn start 작동안됨
$ yarn run start
yarn run v1.22.19
error Command "start" not found.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.

=> 패키지 제이슨에 다음과 같은 스크립트를 써줬음
  "scripts": {
    "start": "react-scripts start"
  },

=> 다시 스타트
 $ yarn start    
yarn run v1.22.19
$ react-scripts start
/bin/sh: react-scripts: command not found
error Command failed with exit code 127.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
=> yarn add react-scripts 해야됨

react모듈이 없다는 오류
$yarn add react
