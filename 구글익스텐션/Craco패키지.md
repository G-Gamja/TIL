craco설명과 함께 개발과정을 다룬 참고 사이트:  https://blog.logrocket.com/creating-chrome-extension-react-typescript/

Craco(Create React App Configuration Override)는 보다 쉽게 CRA 설정이 가능하도록 지원해주는 라이브러리입니다.
출처: https://freestrokes.tistory.com/167 

익스텐션에서는 cra설정을 오버라이딩해서...콘텐츠 스크립트를 ts로 작성해서 웹팩으로 번들링 할 수 있게 할라고 사용하는 것이다.

We already learned that content scripts are special JavaScript files that run within the context of web pages, and these scripts are different and isolated from the React application.

However, when we explained how CRA builds our code, we learned that React will generate only one file with the application code. So how can we generate two files, one for the React app and another for the content scripts?

I know of two ways. The first involves creating a JavaScript file directly in the public folder so it is excluded from the build process and copied as-is into the output. However, we can’t use TypeScript here, which is very unfortunate.

Thankfully, there’s a second method: we could update the build settings from CRA and ask it to generate two files for us. This can be done with the help of an additional library called Craco.

CRA performs all the magic that is needed to run and build React applications, but it encapsulates all configurations, build settings, and other files into their library. Craco allows us to override some of these configuration files without having to eject the project.