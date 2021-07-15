# 7.15 TIL

## wysiwyg

위즈윅
what you see is what you get

github md문제 raw누르면 md코드가 나옴.

## markup

=>마크로 둘러쌓인 언어(태그로)

## markdown

* 타이포라 ctrl+/(마크다운 전환가능 - 보이는대로 작성해도 markdown으로 쓸 수 있음.)
* 마크업의 일종. 
* Perl 스크립트를 통해 HTML파일로 변경 가능
* 페이스북 태그기능( @___)도 마크다운 언어.
  (@가 붙은문자열을 자동으로 태그로 인식하는언어)

### #:HTML의 <h1>~<h6> 붙히고 싶은 만큼 붙여서 쓰면 됨.

(타이포라 ctrl + 1~6로 #안치고 할 수 있음)
(하위속성은 하나씩 낮춰나가기)(글씨크기 맘에 안든다고 건너뛰지 말것)

* 순서가 없는 리스트 만들 수 있음.(탭으로 안쪽으로 들여쓴 목록)

1. 2. 3. ...:순서 있는 리스트(탭 안쪽으로 들여쓴 목록)
         *[글씨]*: 기울인 글씨
         **[글씨]** : 굵은 글씨(ctrl + b눌러도 됨(타이포라에서))
         [링크내용](링크주소)
   3. 오오
      1. tab누르면 안족목록으로

목록에서 나오고싶을때는 shift + tab

shift는 보통 명령어에서 reverse의 의미(ex. ctrl + shift + z)

>오호 인용구는>

`리스트는 백틱`

```python
​```[쓰고싶은 언어]
a = [1,2,3]
s = 'string'
블럭 네임에 쓰고싶은 언어 넣으면 그 문법대로 색 생김.
```

- [ ] 해야할 일

- [X] 완료한 일

  

| 내용 | 정리                        | 요약 | 사용 |
| ---- | --------------------------- | ---- | ---- |
| 오   | \|파이프로표만들기 가능\|   | ㅇㅇ | ㅇㅇ |
|      | ctrl + enter로 열 추가.     |      |      |
|      | 오른쪽 위 속성에서 설정가능 |      |      |



수식작성은 latax(레이텍)
$$
달라두개로 레이텍 문법 작성 가능
(따로 공부하기)
$$

GitHub supports emoji!
:+1: :sparkles: :camel: :tada:
:rocket: :metal:  :octocat:



> emoji에서 이모티콘 가져올 수 있다.  iemoji.com

https://github.com/jinkyukim-me/markdown_ko#11-emoji-%EC%9D%B4%EB%AA%A8%ED%8B%B0%EC%BD%98

에서 마크다운 공부



## 터미널

### 중요한 것은 우리가 탐색기(GUI(Graphic User Interface))에서 할 수 있는 모든 행동을 터미널(CLI(Command Line Interface))에서 동일하게 할 수 있음.

* cd : change directory
* mkdir: make directory
* 위로 올리기 ctrl + l

* 터미널 자동완성 : cd M상태에서 tab누르면 갈수있는 디렉토리 나옴.
* touch a.txt : 텍스트 파일 생성.
* cp a.txt a-copy.txt => a를 a-copy로 복사함.
* rm -rf [파일,폴더명] => remove
* cd ~ (홈으로 감)

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

* git config --global user.name 
* git config --global -l  : 이거로 설정 확인.
* git log : commit된 내용을 확인.
* git remote [repo주소]
* git remote -v로 확인

## TIL(today i learn)

