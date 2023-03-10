# Error [ERR_REQUIRE_ESM]

i18next-scanner: Error [ERR_REQUIRE_ESM]: require() of ES Module /Users/ahnsihun/Workspace/cosmostation-chrome-extension/node_modules/chalk/source/index.js from /Users/ahnsihun/Workspace/cosmostation-chrome-extension/i18next-scanner.config.js not supported.
Instead change the require of index.js in /Users/ahnsihun/Workspace/cosmostation-chrome-extension/i18next-scanner.config.js to a dynamic import() which is available in all CommonJS modules.


해결: 기존에 사용하던 chalk는 i18패키지 안에 있던 chalk를 가져와서 사용했는데 새로 squid를 설치하면서 패키지 제이슨에 chalk가 명시 되면서 node가 그걸 사용하겠다고 인식을 해버려서 버젼이 안맞아 생겼던 문제였음 => 그냥 기존에 chalk를 사용하는 부분을 지워버림