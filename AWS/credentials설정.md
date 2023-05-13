# credential이 저장되는 장소

AWS CLI는 aws configure를 사용하여 지정하는 민감한 자격 증명 정보를 홈 디렉터리의 credentials라는 폴더에 있는 .aws라는 로컬 파일에 저장합니다. aws configure를 사용하여 지정하는 덜 민감한 구성 옵션은 config라는 로컬 파일에 저장되며, 홈 디렉터리의 .aws 폴더에도 저장됩니다.

참조: https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/cli-configure-files.html

AWS CLI설치방법
참조: https://devlos.tistory.com/37
$ brew awscli
참조2: https://dev.classmethod.jp/articles/setting-up-the-aws-cli-environment-in-a-mac/

# 프로필 설정

참조: https://stevenshim.github.io/awscli-profile/

여러 프로필 설정을 할 수 있음

// aws cli를 설치한 후 해당 명령어 사용가능
$ aws configure --profile <여기에프로필이름>  
한 후 iam값 등록해주면 됨

# 발생할 수 있는 오류들

s3에 안올라간다? 이럼 aws credential설정이 잘못됐거나 프로필설정이 안되어있는거임. 체크하셈
-> s3업로드 단계에서부터 `access failed`라고 나옴
