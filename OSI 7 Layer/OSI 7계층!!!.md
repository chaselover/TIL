# OSI 7계층!!!

## 1계층 물리계층(Pysical Layer)



![img](https://media.vlpt.us/images/pbg0205/post/4a7b7311-8328-42d3-b337-7abb587d8af8/image.png)



### -두 컴퓨터가 통신하려면?

* **모든 파일과 프로그램은 0과 1의 나열이다!**

* 결국 0과 1만 주고 받을 수 있으면 된다!
* 

![img](https://media.vlpt.us/images/pbg0205/post/65245405-23e4-458a-83a3-ebd10f963044/image.png)



전성을 통해 +5V를 흘려보내면 1이 보내지고 -5V를 보내면 0이 보내진다고 가정하면

전선만 있으면 모든 data를 주고 받을 수 있다!.



하지만 실제로는 잘 되지 않았다.

 왜?

전자기파는 싸인 함수를 그리기 때문에 

주파수는 1초당 진동한 횟수.(Hz)

![img](https://media.vlpt.us/images/pbg0205/post/a274364a-09f4-48da-8196-0f1016302935/image.png)



문제는 전자기파는 완전한 sin함수를 그리지않고 대충 그린것처럼 나옴

주파수가 계속 바뀌고 전선은 모든 주파수를 통과시키지 못함(모든 매질이 마찬가지)

ex. 어떤 전선은 5-8Hz만 통과가능한데 전자기파가 1~10Hz라면?

-> 손상되어 엉뚱한 데이터(신호)가 도착하게 됨.



![img](https://media.vlpt.us/images/pbg0205/post/9b9db9a2-3a54-4dbf-a871-efe2a429fe79/image.png)



=> 그럼 어떻게 0,1,0,1 신호를 보내야 할까요?

 -> 그냥은 보낼 수 없음(수직 수평으로 이루어진 전자기파는 무한대 HZ까지 범위가 늘어나 전선이 받을 수 없음.)

-> 아날로그 신호로 바꿔서 전선으로 보내야함.

-> 수신컴퓨터는 아날로그 신호를 해석해서 0,1로 바꿔야함.



### 결론 : Physical Layer란?

* 0과 1의 나열을 아날로그 신호로 바꾸어 전선으로 흘려 보내고(encoding)
* 아날로그 신호가 들어오면 0과 1의 나열로 해석하여(decoding)
* 물리적으로 연결된 두 대의 컴퓨터가 0과 1의 나열을 주고 받을 수 있게 해주는 모듈(module)(모듈 = 함수)



![img](https://media.vlpt.us/images/pbg0205/post/4d2a7672-f2d9-40ee-a1c8-55268eedc6e9/image.png)



* 1계층 모듈은  PHY칩으로 하드웨어로 구현되어 있다
* 인코더, 디코더는 함수(input output은 각각 디지털신호(data), 아날로그 신호(signal))! 형태를 갖춘 하드웨어 회로!





---



## -여러대의 컴퓨터 간의 통신.(2대가 아닌)

* 컴퓨터마다 전선을 물리적으로 연결 할 수가 없음.

* 전선을 꽂을 구멍도 없고 전선도 없고 돈도 없음(비용 비효율 초래)
* 1계층의 한계



![img](https://media.vlpt.us/images/pbg0205/post/1555a516-4c01-4fb8-981a-d8c6a06c160e/image.png)



### 따라서 전선 하나로 여러대의 컴퓨터와 연결 시켜야함.

=> 통신망에 구리선을 뽑아 각 컴퓨터에 연결하는 방식.(모든 컴퓨터로 아날로그 신호(전기신호)를 전송하는 방식.)



![img](https://media.vlpt.us/images/pbg0205/post/c078eb46-9536-4e77-9dff-e55bf61a4640/image.png)



문제는 다른 컴퓨터들도 data를 받는다는것!

=> 전선을 뽑아  상자에 넣어볼까?(더미 허브)



![img](https://media.vlpt.us/images/pbg0205/post/47c0db6b-2bcd-4d27-a729-95c275aca7ec/image.png)



=> 상자가 메시지의 목적지를 확인해 타겟에게만 주게 하는 똑똑한 상자 = 스위치(일종의 컴퓨터)

=> 하나의 네트워크의 완성 = 인트라넷(폐쇄망)



![img](https://media.vlpt.us/images/pbg0205/post/ea61da9e-195e-4114-bf0a-e38bb837031c/image.png)



### 만약 둘이 다른 스위치(인트라넷)상에 존재한다면?

=> 서로 다른 네트워크끼리 통신이 가능하게끔 한 장치 = 라우터(Router)

=> 상자는 스위치 + 라우터의 역할을 하게됨(L3스위치)

=> 즉 공유기



![img](https://media.vlpt.us/images/pbg0205/post/cecb0510-250f-401f-b725-8b0cdcf9dc1b/image.png)

라우터들끼리 연결해 전 세계의 컴퓨터를 계층구조로 연결 한 것을 인터넷(Internet)이라 함.

(해저케이블이 전세계를 잇는다.)



---



연결만 됐다고 다가 아니다.

여러대의 컴퓨터가 신호를 보냈을 때 데이터가 섞일 가능성이 있다.

### +데이터가 끊어서 오면 이걸 잘 이어 붙혀 해석 할 수 있을까?

![img](https://media.vlpt.us/images/pbg0205/post/48763274-a593-49e8-9533-bdceebd6c479/image.png)

0000과 1111을 찾아 데이터를 분할해 잘 구분해석 할 수 있다.

(flag) -> 디코딩이 최후에는 flag를 제거하고data원본을 받을 수 있음.







## 결국 2계층 Data-link Layer 란?

* 같은 네트워크에 있는 여러대의 컴퓨터들이 데이터를 주고받기 위해 필요한 모듈
* Framing(프레이밍)은 Data-link Layer에 속하는 작업들 중 하나입니다.
* 3계층과의 차이는 동일한 네트워크 내에서 전송(3계층은 네트워크와 네트워크 간.)
* 오류제어, 흐름제어.
* 2계층의 오류제어는 손상된 Frame을 버리는 식(4계층은 손실에 대한 요구 및 오류복구까지 시행.)

![img](https://media.vlpt.us/images/pbg0205/post/d8e1d73d-aeac-4176-84ad-f60d68c964e7/image.png)

* 2계층도 1계층 모듈처럼 랜카드에 하드웨어적으로 구현되어 있음.
* 스위치는 캠테이블을 통해 스위치에서 목적지인 B로 전송하게 됨.
* 패킷에 보내는 컴퓨터의 맥주소와 가장 가까운 라우터의 맥주소를 넣어 1계층으로 보냄.
* dhcp, arp를 통해 라우터 IP를 맥주소로 변환한 후에 라우터에 도한 도착지 맥어드레스를 만든 후 헤더에 넣어줌.
* 특이하게 앞에 trailer라는 정보도 붙게 되는데 이는 오류제어를 위한 정보이다. 이걸 캡슐화 한 걸 Frame이라 한다.





---



## Network Layer(3계층)

![img](https://media.vlpt.us/images/pbg0205/post/d2f9e7ef-d2b5-4a4e-9d1d-e1cf58d3120c/image.png)

* A에서 B로 데이터를 전송하고 싶은 경우 A컴퓨터에 B의 주소를 담아서 데이터를 전송한다.

* 이 때 입력하는 주소는 각 컴퓨터들의 고유한 주소를 가지게 되는데 이를 IP 라고 한다.

  

### 그럼 A는 B의 주소를 어떻게 알게 되는가?(알아야 보낼 수 있다.)



1. 우리가 주소창에 www.naver.com을 입력하면

2. 이 영어주소는 IP주소로 변환되어 사용된다.(동치관계)

3. 이를 DNS(Domain name system)이라 한다.

4. 따라서 우리는 www.naver.com의 IP주소를 알고있는 것이나 다름 없다.

   ++[{ip주소 + data}]형태를 패킷(packet)이라 한다.



### 패킷은 어떻게 이동되는가?

1. 라우터 '가'는 패킷을 열어 목적지를 확인한다.
2. 목적지 IP가 나에게 없다?
3. 패킷을 '마'로 보냄(자신과 연결되어 있는 유일한 라우터)
4. '마'도 동일 과정.
5. 하지만 '마'는 연결된 라우터가 너무 많아 어디로 보내야할지 헷깔림ㅜ.ㅜ
6. ...라우팅을 공부해야한다.

(디캡슐레이션 두번을 통해 도착지 IP주소를 확인한 후 라우터에서 

라우팅 테이블을 통해 라우팅 시켜 도착지에대한 맥주소로 업데이트 시킴.)

(수많은 라우터들에 대해 업데이트 되면서 전달하게 됨.)



## 결국 Network Layer(L3)란?



* 수많은 네트워크들의 연결로 이루어지는 inter-network 속에서
* 어딘가에 있는 목적지 컴퓨터로 데이터를 전송하기 위해
* IP 주소를 이용해서 길을 찾고(routing)
* 자신 다음의 라우터에게 데이터를 넘겨주는 것(forwarding)



![img](https://media.vlpt.us/images/pbg0205/post/9d2a6200-9f19-4681-9256-d5314fa62c4f/image.png)



##### 3계층 인코더 -> 2계층 인코더

패킷 안의 data = 구조체 = 객체(안에 필드가 있고 변수 중 하나에 데이터를 넣고 또 다른 변수를 만들어 목적지 주소를 받는 방식)

##### 2계층 -> 1계층 인코더

farming이 된 데이터(01010101010)

##### 1계층 인코더 -> 라우터

아날로그신호



세그먼트에 도착지와 출발지에 대한 정보를 붙히고 캡슐화 한것이 패킷.



### Network기술은 어디에 구현되어 있을까?

* 운영체제의 커널에 SW적으로 구현되어 있다.



---



## Transport Layer(L4) 전송계층!



이제 인터넷 상의 모든 컴퓨터가 서로와 통신 할 수 있게 되었는데!

하지만 컴퓨터는 여러 프로세스들을 수행하고 있다!

우리가 받은 데이터를 프로세스 들에게 나누어 줘야한다.

++세그맨테이션, 흐름제어, 오류제어를 제공

* 세그맨테이션 : 상위계층 데이터를 받아 세그멘트라는 단위로 나누는 것을 의미.(동영상 스트리밍을 가능하게함)

(일부분씩 데이터 전송을 통한 스트리밍 및 데이터 손실률 줄임)

* 흐름제어 : 초당 10mb 처리할 수 있는 기기에 50m 또는 5mb를 전송했을 시 속도변경에 대한 요청을 하는 것

(Stop and Wait or Sliding Window를 사용.)

* 오류제어 : 내가 본내 데이터가 오류손실이 없는지? 오류가 있다면 해당 데이터를 다시 보내주는 것을 의미

  (FEC, BEC, ARQ 등 방식이 있음.)



### 어떤 데이터를 어떤 프로세스에 주어야 하는지 컴퓨터가 어떻게 알 수 있는가?

* data를 받고자하는 프로세스들은 Port번호를 가져야함.
* Port 번호는 하나의 컴퓨터에서 동시에 실행 되고 있는 프로세스들이 서로 겹치지 않게 가져야하는 정수 값입니다.
* 송신자는 data를 보낼 때 데이터를 받을 수신자 컴퓨터에 있는 프로세스의 포트 번호를 붙여서 보낸다.(데이터 앞에)



#### 그럼 송신자는 수신자가 가진 프로세스의 포트번호는 어떻게 알지?

검색창에 www.naver.com을 입력하는 것은 사실 뒤에 :80이 생략되어 www.naver.com:80 이다.

즉 우리는 이미 포트번호를 알고있는것!(도메인의 포트번호)

![img](https://media.vlpt.us/images/pbg0205/post/c6698b2f-bb27-4b92-b480-7ce3999389ea/image.png)



![img](https://media.vlpt.us/images/pbg0205/post/a2b43633-c77e-4b35-8adf-b8d5571d801a/image.png)

그럼 데이터를 뜯어 포트번호에 맞는 프로세스에 data를 전달 할 수 있다.



## 결국 Transport Layer란?

* Port 번호를 사용하여
* 도착지 컴퓨터의 최종 도착지인 프로세스에 까지
* 데이터가 도달하게 하는 모듈
* 운영체제(OS)의 커널에 SW적으로 구현되어 있다.

![img](https://media.vlpt.us/images/pbg0205/post/7b09fdaa-222c-4368-b103-58b6fa6ff435/image.png)

전체 흐름.

(TCP를 슬꺼냐 UDP를 쓸꺼냐도 정하는 계층)

TCP는 제가 보낸 데이터를 손실이 되었는지 확인하고 데이터의 순서도 보장하기 때문에 조금 더 신뢰적임.

UDP는  데이터를 보내고 나면 책임을 지지 않음. 즉 신뢰도는 떨어지나 속도가 빠르고 연속적(스트리밍에서씀)

Transport 헤더에선 tcp인지 udp인지에 대한 정보와 출발지와 도착지에 대한 포트정보를 헤더에 넣어 뒤에 붙인 후 캡슐화 해줌.

이 결과물을 세그멘트(segment)리고 부름.

---



## Application Layer(L7)



![img](https://media.vlpt.us/images/pbg0205/post/4b6c0184-d38f-44ef-a732-7de5c44778c7/image.png)

갑자기 7계층??

현대의 인터넷 모델은 OSI모델이 아니라 TCP/IP 모델을 따르고 있기 때문.

* 현대의 인터넷이 TCP/IP  모델을 따르는 이유는 OSI 모델이 TCP/IP 모델과의 시장 점유 싸움에서 졌기 때문.
* 또 TCP/IP 모델에 물리영역이 업데이트되어 OSI 모델과도 비슷해졌기 때문이다.
* HTTP, FTP, SMTP, Telnet등이 속한 계층.



### TCP/IP Socket 프로그래밍

* 운영체제의 Transport Layer에서 제공하는 API를 활용해 통신 가능한 프로그램을 만드는 것을 TCP/IP 소켓 프로그래밍, 또는 네트워크 프로그래밍 이라고 합니다.

소켓 프로그래밍 만으로도 클라이언트, 서버 프로그램을 따로따로 만들어서 동작 시킬 수 있다.

또 TCP/IP 소켓 프로그래밍을 통해서 누구나 자신만의 Application Layer 인코더와 디코더를 만들 수 있다.

=> 즉 누구든 자신만의 Application Layer 프로토콜을 만들어서 사용할 수 있다는 뜻입니다.



### 대표적인 Application Layer 포로토콜인 HTTP의 인코딩&디코딩

* HTTP 이해를 위해선 클라이언트 & 서버 패러다임을 알아야 한다.
* header, body, request, response, status code 등..

![img](https://media.vlpt.us/images/pbg0205/post/cf3d53a2-e90c-45c7-b5d6-7400b5522baf/image.png)

1. HTTP 인코더로 {status code + 잡다한거 + data}
2. 4계층 인코더로 Port num + {status code + 잡다한거 + data}
3. 1~3계층을 통해 나온 아날로그 신호가 다시 클라이언트 컴퓨터로 전달되어 역순으로 디코딩



---

MVC패턴은 소프트웨어 아키텍처 중 하나

MVC와 마찬가지로 소프트웨어 아키텍처 중에 Layered Architecture 가 있다.

레이어드 아키텍쳐를따르는 대표적인 예가 네트워크 시스템이다.

즉 네트워크 시스템은 하나의 커다란 소프트웨어라고 할 수 있다.

결론적으로 OSI 7 Layer는 거대한 네트워크 소프트웨어의 구조를 설명하는 것이다.

(모델 이해를 위한 모델이라고 보면 됨.)

(국제표준기구 ISO가 발표한..)





---

+++ 6계층은 데이터의 변환, 압축, 암호화가 이루어 지는 계층(장비별로 서로 다른 인코딩을 사용할 수 있기 때문.)



+++ 5계층 세션을 열고 닫고를 제공하는 매커니즘의 계층(세션 복구도 제공)

세션 도커는 체크포인트를 통해 동기화를 시켜줌.(100MB전송 시 5MB마다 체크포인트 설정하면 중간에 끊겨도 다음에 이을 수 있음.)