# 웹 크롤링이란?

**조직적, 자동화된 방법으로 웹을 탐색하는 것.**

=> 데이터(API) 최신 상태 유지를 위해 웹 크롤링을 한다.



* 요청(request)은 URL 
* 응답(response)은 Text 덩어리(문서)
	* (딕셔너리를 받는다고 말하면x string덩어리(HTML,XML,JSON))
	
	  

---

## 검색



> https://www.google.com/search?q=ssafy&rlz=1C1CHZN_koKR942KR942&oq=ssafy&aqs=chrome..69i57j69i59l3j69i60l4.1821j0j15&sourceid=chrome&ie=UTF-8

**google에 싸피를 검색한 결과이나 실질적으로**

> https://www.google.com/search?q=ssafy

**이거만 검색해도 동일한 페이지(주소)를 보여준다.**



크롬이 주소에 대한 요청을 보내면 

그 응답결과를 웹 화면으로 보여주는 방식



---

###  request 패키지

```bash
pip install request
```


pip못 찾는다하면 

```bash
python -m pip install request
```



```python
import requests

requests.get(URL)
requests.get(URL).text
requests.get(URL).status_code
```

URL에 요청을 보내서 정보를 get함.

														해서 text만 뽑아옴.
	
														상태(status_code)만 뽑아옴



---

## 정보스크랩

1. 정보가 있는 사이트 URL을 확인한다.

2. URL로 요청을 보낸다.

3.  응답 결과에서 원하는 정보를 찾는다.

   (응답결과 객체저장 후 탐색)



---

## BeautifulSoup4 패키지
```bash
python -m pip install beautifulsoup4
```

```python
from bs4 import BeautifulSoup

BeautifulSoup(받아온 문서)   			  //받은 문서(다발)을 보기 좋고, 검색하기 좋게 만들어줌
BeautifulSoup.select(경로(selector))		//문서안에 있는 특정 내용을 뽑아줌(select)
BeautifulSoup.select_one(경로)			//특정내용 하나만 뽑음.
```

response를 예쁘게 관리하는 패키지.



1. 원하는 정보를 가진 페이지에서 검사를 때린다. 

2. CSS 선택자(selector)를 가져온다.(복사한다)

   	css선택자는 HTML을 가져오기 위한 수단(원하는 data만.)

3. 이걸 BS가 구조화해줌.

---

## JSON(JavaScript Object Notation)

데이터만을 주고 받기 위한 표기법 - XML도 있음.

보통 API를 가지러 들어가면 date는 json형식을 띔.->Table 형태로 Dictionary와 list를 써 쉽게 변환.

---

## API(서비스간 프로그래밍을 통한 소통방식)

1. API key 값 확인.

2. 요청 변수에 맞춰 값을 넣음
3. JSON(XML)형식의 결과를 받아 확인.