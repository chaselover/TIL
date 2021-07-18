# Flask를 이용한 API활용.

### 추후복습시 Root디렉토리 안 flask서버 py파일 참고할 것.

주소는



URL(요청)

'?'

method(속성)



로 이루어진다.



**모든 URL의 존재이유는 "요청(requests)"**

​	API는 늘 API제공자가원하는대로 요청을 보내줘야함(URL형식)(매우중요) 

​	=> 늘 공식문서(document)를 꼼꼼히 읽어야함.

​	(ex. https://core.telegram.org/bots/api#setwebhook)

​	(ex. https://developers.naver.com/apps/#/list)

​	(ex. https://developers.naver.com/docs/serviceapi/search/shopping/shopping.md#%EC%87%BC%ED%95%91)



다음과 같은 내용으로 보아 API는 Cli 시크릿값 or 인증을 통한 접근 토큰

같은 걸 필요로 함.

```
비로그인 방식 오픈 API는 HTTP 헤더에 클라이언트 아이디와 클라이언트 시크릿 값만 전송해 사용할 수 있는 오픈 API입니다. 네이버 아이디로 로그인의 인증을 통한 접근 토큰을 획득할 필요가 없습니다.
```

* GET  =>  받는거.(URL로 요청을 보내면)

* POST  =>  줄테니 받아서 처리해줘라

+++XML -> JSON(JSON이 더쌈.)

---


```
-요청변수는 늘 ? 붙히고 뒤에 변수=꼴로 넣음.

-requests, respons Headers는 key:value들로 이루어져있음

-id, code를 보내려면 requests Header에 두개를 껴서 보내야함.

-그러나 웹 ''브라우저''에서는 불가능함. => 서버에서 전송 시 붙혀서 보내야함.
```



## 비로그인 방식 오픈 API 호출 예시 中

```java
con.setRequestMethod("GET");
con.setRequestProperty("X-Naver-Client-Id", clientId);
con.setRequestProperty("X-Naver-Client-Secret", clientSecret);
```

를

```python
import requests
from pprint import pprint

keyword = '마카롱'

url = f'https://openapi.naver.com/v1/search/shop.json?query={keyword}'

naver_client_id =''
naver_client_secret = ''

headers = {
    'X-Naver-Client-Id': naver_client_id,
    'X-Naver-Client-Secret':naver_client_secret,
}

# 쇼핑 검색 API 호출에 필요한 get 메소드 
res = requests.get(url,headers = headers).json()
pprint(res)
```

이렇게하면 검색결과 API줌

pprint는 json파일 정리해서 이쁘게 출력해줌.

---

+++네이버같은 사이트는 robot.txt에서 크롤링을 제한함.

(크롤링규칙이 적혀있음. 모든 사이트에 대해 disallow)

=>트래픽문제때문에/

---



일의 단위 = 함수

=> 기능이 분리되어있다면 모듈단위(함수단위)로 설계할수있음.



---

## 하위 디렉토리 모두 B위치로 복사.

```
$cp -r ./상품검색봇 /mnt/c/Users/한승주
```


---



#### Response의 방식

1. 메세지가오면
2. 어디론가??(우리의 URL??우리 요청어떻게 받아?)
3. 요청을 보내(URL)



대접한다는 영어로 serve -> server

요청한다. ->client (웹 브라우저)

=> 응답하는 프로그래밍 = 서버 프로그래밍.(응답해주는 모든 컴퓨터 = 서버 컴퓨터)



## Flask로 서버구축
```
python -m pip install flask
```

=>flask 서버 만든다.



=>telegram 에서 요청 받으려면 setWebhook을 해야함.(내부 정보를 우리에 보내주는)

---

## 도메인과 IP



모든 도메인은 IP가 있다.


```
IP 서울특별시 ㅇㅇ구 ㅇㅇ동 몇 번지 몇 호

Domain 역삼 멀티캠퍼스
```


우리 주소는 어디야?

127.0.0.1=>이건 '우리집'이라는 뜻.

(외부에서는 우리집이 어딘 지 알 수 없다.)



## Ngrok이용 우회주소 설정.

서버컴퓨터는 외부요청을 받을 수 있으나

개인 컴퓨터는 외부 요청을 다 막아버림(방화벽)

-> 백도어를 깔아서 우회해서 우리컴퓨터로 들어올 수 있게 주소를 하나 팔것임. ->ngrok



=> ngrok(내 서버가 아닌 다른서버)을 통해 

우회해서 내 컴퓨터로 들어올 수 있도록 함

```bash
ngrok http 5000
```

우리는 5000번이라는 포트(항구)를 쓰겠다고 설정함.\

=> 안되면 ngrok직접오픈.



ngrok주소(우회 가능한 URL) -> NGROK이 중계를 해서

내 컴퓨터의 5000번에 돌고있는 flask에 포워딩해줌.(forwarding)



---

API에 ngrok setWebhook으로 접속. 연결되나 확인.

```python
if __name__  == '__main__':
    token={API접근 토큰 주소}
    print(f'https://api.telegram.org/bot{token}/setWebhook?url=[Ngrok Forwarding 주소]')
```

=> setWebhook 타겟 목적지 우회 서버로 설정하면



```python
# 프린트 되는 Request객체 안에는 /주소안의 모든 정보가 들어있음.
#  flask request 공식문서 참조.
@app.route('/')
def index():
    print(request.host)
    return 'Hello!!'


# 우리가 서버로써 받은 request를 해체하는 곳이 request.get_json()(요청을 받겠다.)
@app.route('/recieve', methods = ['POST'])
def recieve():
    print(request.get_json())
    return 'ok', 200
```

POST 메소드로 request.get_json()받아짐.

---

server의 Webhook안에 상대가 받은 모든 msg를ngrok주로소 보내고. 이걸 ngrok이 127.0.0.1로 포워딩 시킹.

이걸 내 서버가 받아

@app.route(''/recieve')주소가 오면

recieve함수를 실행해 send_message를 함.



## Client -> Server

#### (딱 정해져있는게 아니라 요청하면 Client고 받으면 Server다.)

(한번의 과정에도 여러번 client와 server가 바뀜.)



---

https://www.pythonanywhere.com/user/hansj/webapps/#tab_id_hansj_pythonanywhere_com

파이썬 애니웨어 이용 내컴퓨터를 켜놓고 있지 않아도 되도록 클라우드 베포함.

