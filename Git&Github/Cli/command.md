
## 만나서 반갑습니다

## 안녕하세요
__강조입니다__ 
### 깃허브에 파일명 & 폴더명 변경 반영하기
* 변경을 원하는 폴더 & 파일의 상위 폴더에 접근하기
* 터미널 창에 cd {해당 폴더의 경로값}
* git mv {oldName} {newName}
//단순 대 소문자 변경으로 인해 오류가 발생하는 경우 직접 변환 대신 temp등의 임시 이름을 거쳐갈 것

### 원격 branch 가져오기

https://cjh5414.github.io/get-git-remote-branch/

git remote update : 원격 브랜치에 접근하기 위해 git remote 갱신
원격의 브랜치를 찾지 못해서 발생하는 fatal: Cannot update paths and switch to branch 'feature/rename' at the same time. 라는 오류 메세지를 해결해준다.

$git branch -r: 원격 저장소의 브랜치 리스트
$ git branch -a:로컬, 원격 모든 브랜치 리스트
$ git checkout -t 브랜치:  
-t 옵션과 원격 저장소의 branch 이름을 입력하면 로컬의 동일한 이름의 branch를 생성하면서 해당 branch로 checkout을 한다.

만약 branch 이름을 변경하여 가져오고 싶다면 $ git checkout -b [생성할 branch 이름] [원격 저장소의 branch 이름] 처럼 사용하면 된다.


git branch -t 브랜치명
git pull origin 브랜치명