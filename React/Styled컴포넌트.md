#스타일드 컴포넌트
참조: https://www.daleseo.com/react-styled-components/
## CSS in JS

CSS in JS는 스타일 정의를 CSS 파일이 아닌 JavaScript로 작성된 컴포넌트에 바로 삽입하는 스타일 기법입니다.

기존에 웹사이트를 개발할 때는 HTML과 CSS, JavaScript는 각자 별도의 파일에 두는 것이 best practice로 여겨졌었습니다. 하지만 React나 Vue, Angular와 같은 모던 자바스크립트 라이브러리가 인기를 끌면서 웹개발의 패러다임이 바뀌고 있습니다. 최근에는 웹 애플리케이션을 여러 개의 재활용이 가능한 빌딩 블록으로 분리하여 개발하는 컴포넌트 기반 개발 방법이 주류가 되고 있습니다.  

따라서, 웹페이지를 HTML, CSS, JavaScript 3개로 분리하는 것이 아니라, 여러 개의 컴포넌트로 분리하고, 각 컴포넌트에 HTML, CSS, JavaScript를 몽땅 때려 박는 패턴이 많이 사용되고 있습니다. React는 JSX를 사용해서 이미 JavaScript가 HTML을 포함하고 있는 형태를 취하고 있는데, 여기에 CSS-in-JS 라이브러리만 사용하면 CSS도 손쉽게 JavaScript에 삽입할 수 있습니다.

