# dotenv

cf. create-react-app에서 환경변수 사용하는 경우
create-react-app에는 이미 dotenv 패키지가 내장되어 있다.

따라서 별도의 패키지 추가나 Webpack에 대한 설정 없이 .env 파일을 생성해서 변수를 선언하는 것만으로도 환경변수를 사용할 수 있다.

하지만 내부적으로 REACT*APP*으로 시작하는 환경변수만 읽어들이도록 설정을 해두었기 때문에 변수명은 반드시 REACT*APP*으로 시작해야 한다!
