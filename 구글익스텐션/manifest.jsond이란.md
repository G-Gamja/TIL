manifest.json
manifest.json 파일은 json 포맷 파일로서, 모든 웹 익스텐션이 포함하고 있어야 하는 파일입니다.

manifest.json을 사용함으로써, 당신은 당신의 익스텐션의 이름, 버젼과 같은 기본 정보를 명시하며, 또한 당신의 익스텐션의 기능, 예를 들어 기본 스크립트, 내용 스크립트, 브라우져 활동 등과 같은 측면을 명시합니다.

https://developer.mozilla.org/ko/docs/Mozilla/Add-ons/WebExtensions/manifest.json

- name: this is the name of the extension
- description: description of the extension
- version: current version of the extension
- manifest_version: version for the manifest \* format we want to use in our project
- action: actions allow you to customize the buttons that appear on the Chrome toolbar, which usually trigger a pop-up with the extension UI. In our case, we define that we want our button to start a pop-up with the contents of our index.html, which hosts our application
- icons: set of extension icons

# Manifest V3

https://blog.betaman.kr/104
