# Web storage

웹 스토리지는 서버가 아닌 클라이언트에 데이터를 저장할수 있도록 하는 HTML5의 새로운 기능이다.

WS API와 쿠키의 기능자체는 같지만 쿠키는 4KB, 웹스토리지는 5MB까지 가능한다.



* localStorage : 브라우저를 종료해도 데이터가 남아있는 저장소
* sessionStorage : 브라우저를 종료하면 데이터가 소멸되는 저장소



## localStorage

* 지속적으로 필요한 데이터(자동로그인) 등에 쓰인다.
* 저장 한도는 3가지 중 가장 높다.



사용자 현재 도메인의 localstorage에 접근해 localstorage의 setitem메서드를 사용하여 데이터를 추가합니다.

```js
localStorage.name="랄라라라라"
localStorage['name'] = '랄라라라라'

localStorage.setItem('name','랄라라라라')
```



```js
localStorage.getItem('name') //랄라라라라
	localStorage에 저장된 아이템 읽기

localStorage.removeItem('name')
삭제

localStorage.clear();
다 삭제
```



```js
const items = JSON.stringify([1,{a:'b'},3,4,5]);
localStorage.setItem('item',items);

const data = JSON.parse(localStorage.getImtem('items'));
```

Object나 Array도 저장 가능함.



## SessionStorage

페이지 세션이 종료되면 바로 지워진다. 그리고 다시 페이지를 열면 새로운 세션이 생성된다.

localStorage와 동일한 방식을 통해 저장, 삭제가 가능하다.

데이터가 서버로 전송되지 않는다.

일시적으로 필요한정보(일회성 로그인)등에 쓰인다.



---



> storage들은 브라우저마다 지원 범위가 다르고 보통 4~10정도이다.
>
> 장바구니나 좋아하는 컨ㅊ텐츠 등 수시로 변경되는 경우나 글 작성 중간에 임시저장을 구현하는 용도로 쓰인다.



## Cookie

쿠키는 사용자가 웹 사이트 접속 시 개인장치에 다운로드 되고 브라우저에 저장되는 작은 텍스트 파일입니다.

JS를통해 쿠키를 세팅, 가져오고 삭제할 수 있습니다.

쿠키는 실제 서버로 전송되지만 웹 스토리지는 전송되지 않습니다.