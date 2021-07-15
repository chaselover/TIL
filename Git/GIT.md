## GIT

### 수정 및 버전관리, 백업을 하기위한 도구(용량문제나 백업이 안되어 내용 상실을 해결하기 위한)효율적인 버전관리 저장소.

++용량문제 해결을 위해 "변경사항"만 남긴다.(겹치는 부분 다 삭제)

* git init
* git add .

디렉토리를 사진찍기위한 명단

* git commit -m "~~~~"

사진을 찍는다.

* git config --global user.email "~~~"

전자 서명을 만든다.(커밋 작성자 설정)

```Terminal
$ git config --global user.name 
$ git config --global -l  // 이거로 설정 확인.
$ git log // commit된 내용을 확인.
$ git remote [repo주소]
$ git remote -v //로 확인
$ hint: Updates were rejected because the tip of your current branch is behind //에러발생
$ git push -u origin +master //로 강제 push하면 됨.
$ commit log 
$ git reset --soft/mixed/hard [commit주소] //삭제
$ touch README.md
$ start README.md
```