# BOM(Browser Object Model) API

![img](https://media.vlpt.us/images/tjdud0123/post/5fdd4197-125a-4790-b56f-5b0ef03fe7a0/image.png)

<br>

## **BOM** (Browser Object Model)

* **웹브라우저의 창이나 프레임**을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
* 전역객체인 **Window의 프로퍼티와 메소드**들을 통해서 제어
* 웹 브라우저와 관련된 객체의 집합.
* DOM의 일부로 보는 시선도 있음.

<br>

- 전역변수와 함수가 사실은 window 객체의 프로퍼티와 메소드
- 모든 객체는 사실 window의 자식
- DOM과 달리 BOM은 공식적인 표준이 없기에 웹 브라우저마다 BOM API가 조금씩 다를 수 있음
- window는 생략 가능

<br>

### window 메소드.

* [window.innerWidth](https://developer.mozilla.org/en-US/docs/Web/API/Window/innerWidth) : window의 화면 가로 폭
* [window.alert()](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert) : 알림창
* [window.confirm()](https://developer.mozilla.org/en-US/search?q=confirm) : Yes, No 값을 받을 수 있습니다.
* [window.open()](https://developer.mozilla.org/en-US/docs/Web/API/Window/open) : 새창으로 오픈
* [window.close()](https://developer.mozilla.org/en-US/docs/Web/API/Window/close) : 창 닫기
* [window.scrollX = window.pageXOffset](https://developer.mozilla.org/ko/docs/Web/API/Window/scrollX) : 가로 스크롤바 위치
* [window.scrollTo()](https://developer.mozilla.org/ko/docs/Web/API/Window/scrollTo) : 절대적 위치이동
* [window.setTimeout()](https://developer.mozilla.org/ko/docs/Web/API/WindowTimers/setTimeout) : 시간 후에 1회만 함수 실행
* [window.setInterval()](https://velog.io/@tjdud0123/setInterval) : 주기마다 계속 함수 반복
* window.localStorage](https://developer.mozilla.org/ko/docs/Web/API/Window/localStorage) : 로컬스토리지

<br>

#### Window 객체

* **Navigator** : 브라우저 정보
  * [Navigator - MDN](https://developer.mozilla.org/ko/docs/Web/API/Navigator)
  * 웹페이지를 실행 중인 브라우저에 대한 정보가 담긴 객체
  * navigator 객체의 속성
    * navigator.userAgent : 사용자의 브라우저 감지
    * navigator.cookieEnabled : 쿠키 가능여부
    * navigator.onLine : 온라인 여부
    * navigator.language : 언어감별 ex) “ko”

<br>

* **Location** : 주소창 부분
  * [Location - MDN](https://developer.mozilla.org/ko/docs/Web/API/Location)
  * 브라우저의 주소 표시줄과 관련된 객체
  * location 객체는 프로토콜의 종류, 호스트 명, 문서 위치 등의 정보가 있음
  * location 객체의 속성
    * location.href; : 주소
    * location.host; : “github.com”
    * location.hash; : 해당페이지의 목적지(id)
    * host : 호스트 이름과 포트번호 // localhost:30763
    * hostname : 호스트 이름 // localhost
    * port : 포트 번호 // 30763
    * pathname : 디렉토리 경로 // /a/index.html
    * search : 요청 매개변수 // ?param=10
    * protocol : 프로토콜 종류  // http:

<br>

\- location 객체의 메소드

* assign(link) : 현재 위치를 이동

* reload() : 새로고침

* replace(link) : 현재 위치를 이동(뒤로가기 사용 불가)

<br>

* **History** : 이번보기 다음보기 등
  * [History - MDN](https://developer.mozilla.org/ko/docs/Web/API/History)
    * history.back() : 한칸 뒤로가기
    * history.forward() : 한칸 앞페이지 가기
    * history.go(-2) : 2칸 뒤로가기 등 제어가능

<br>

* **Screen** : 디스플레이 부분
  * [Screen - MDN](https://developer.mozilla.org/ko/docs/Web/API/Screen)
  * 웹브라우저 화면이 아닌 운영체제 화면의 속성을 갖는 객체
  * screen 객체의 속성
    * screen.height : 스크린 height
    * screen.availHeight : 실제 사용가능한 height
    * screen.orientation : 모바일 기기 방향

<br>

* **Document** : 웹 페이지 문서의 HTML, CSS 등에 대한 접근
  * document.title : title
  * document.doctype :
  * document.activeElement; : focus된, 활성화된 엘리먼트
  * document.cookie : 쿠키관리

