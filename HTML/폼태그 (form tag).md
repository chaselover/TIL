# 폼태그 (form tag)

user의 의견이나 정보를 알기 위할 때 사용 한다.

입력된 데이터를 한번에 서버로 전송, 전송한 데이터는 웹 서버가 처리하고, 결과에 따른 도 다른 웹 페이지를 보여준다.



> 동작방법

* 폼이 있는 웹 페이지를 방문
* 내용 입력
* 데이터를 웹 서버로 전송
* 서버는 받은 폼 데이터 처리를 위해 웹 프로그램으로 전송
* 데이터 처리
* 처리 결과에따른 새로운 html 웹서버로 전송
* 웹서버는 받은 html페이지 브라우저로 전송
* 브라우저는 받은 html 렌더링



> 폼태그 속성

* action : 폼을 전송할 서버 쪽 스크립트 파일을 지정
* name : 폼을 식별하기 위한 이름 지정
* accept-charset : 폼 전송에 사용할 문자 인코딩 지정
* target : action에서 지정한 스크립트 파일을 현재 창이 아닌 다른 위치에 열도록 지정
* method : 폼을 서버에 전송할 http메소드 지정(GET? POST?)



> Get과 POST차이

* GET은 폼 데이터를 URL끝에 붙여서 눈에 보이게 보낸다.(보안에 취약)

  지정된 리소스에서 데이터를 요청하는 경우(READ)에서 씀

* POST방식은 외부적으로는 보이지 않게 보낸다.

  지정된 리소스에서 데이터를 처리할 경우(Create,Update,Delete) 씀



> 기타 엘리먼트

* fieldset, legend엘리먼트(tag)

* fieldset은 form 내부에서 항목을 묶는 세부 항목으로 분리시킨다.

* legend는 각각 fieldset에서 title을 달아준다.

* input

  * type : text,radio,checkbox,password,button,hidden,fileupload,submit,reset,date(달력)

    number, range(바형태로 좌우로 조절하는 음량조절 느낌), color

  * name : 태그 이름 지정

  * readonly 읽기전용

  * maxlength : 입력 최대 글자수 지정

  * required 필수태그로 지정. HTML5추가사항(에러메세지 보냄)

  * autofocus : 웹 페이지 로딩되자마자 이속성을 지정한 태그로 포커스가 이동됨(HTML5)

  * placeholder

  * pattern : 정규표현식을 사용. 특정 범위에 유효한 값을 입력받을 때 사용(비밀번호규칙등)

* 목록태그

  * select : 항목을 선택 가능한 태그
    * size : 한번에 표시할 항목 수
    * multiple 다중선택 허용할 것인지
    * optgroup : select bar안의 항목들을 그룹화 해 보여줄때 사용
      * label속성 을 이용해 그룸 이름 지정
      * option태그 : 목록을 나타내는 태그(optgroup의 선택지 각 항목들)

* 여러줄 글상자 textarea
  * rows,cols : 몇줄? 한줄에 몇글자? 지정 속성

* datalist : id값 지정한 것을 input 의 list속성값으로 지정해텍스트 박스 아래 그룹 리스트로 출력시킨다. 각항목은 option tag사용. 

