# HTTP1.1&HTTP2&QUIC

![img](https://media.vlpt.us/images/injoon2019/post/6c2565e3-4658-4aeb-9355-b8c201cb2355/image.png)

* HTTP는 Application Layer의 프로토콜.
* HTTP는 신뢰성있는 연결만 해줄 수 있다면 전송프로토콜은 TCP/UDP 뭘써도 상관없다고함.

## TCP(Transmission Control Protocol)

* connection을 수립하기 전에 신뢰성 있는 Connection을 위해 3 Way Handshake를 함.

  ![img](https://media.vlpt.us/images/injoon2019/post/41068ab4-3a02-4468-96fd-00c6584dd738/image.png)

![img](https://media.vlpt.us/images/injoon2019/post/24847a7e-2889-4143-b230-a1757e3518c6/image.png)

3 악수를 통해 연결이 수립되면 데이터를 전송함. 전송하면 잘 받았다고 응답을 보냄(유실되면 다시 데이터 요청)

제어, 핸드셰이커, 등등신뢰성 구축에 신경을 많이씀.

## UDP(User Datagram Protocol)

![img](https://media.vlpt.us/images/injoon2019/post/5ec6901d-85c2-4894-87b9-a1ce575acbca/image.png)

> 신뢰성 x 받는걸 신경을 안쓰고 그냥 보냄.

## TCP vs UDP

![img](https://media.vlpt.us/images/injoon2019/post/ab41d5ed-c13f-4c20-8dbb-6b5e7afb1e41/image.png)

## HTTP

![img](https://media.vlpt.us/images/injoon2019/post/5d577279-d7f8-4c77-82e3-919399a8f6df/image.png)

### 0.9

![img](https://media.vlpt.us/images/injoon2019/post/f2e4e856-4ec0-489e-88b1-43308d582d94/image.png)

요청도 GET밖에없음. only HTML

### 1.0

![img](https://media.vlpt.us/images/injoon2019/post/82e96965-2e63-4efa-836c-a5d965c50849/image.png)

헤더생김, 상태코드, 버전, content-type생김(HTML말고 다른타입도)

![img](https://media.vlpt.us/images/injoon2019/post/6f86967b-5723-4ce7-b3ff-d9f2d26cca08/image.png)

커넥션 하나당 요청하나, 응답하나만 처리할 수 있음. => 요청당 연결을 수행해 성능이 저하됨(삐효율)

![img](https://media.vlpt.us/images/injoon2019/post/d86bb1e2-c08c-47b4-89e8-b7abcc0ae540/image.png)

### 1.1 퍼시스턴트 커넥션

![img](https://media.vlpt.us/images/injoon2019/post/c7409fd0-a88d-499b-ba51-4a7ea3bd53e0/image.png)

커넥션을 열어두면 여러 요청이 이 커넥션을 사용할 수 있음.(지속 커넥션)

단기 커넥션하고 비교해보면 네트워크 사용시간이 훨씬 줄어듦.

#### 파이프라이닝

![img](https://media.vlpt.us/images/injoon2019/post/04be2539-f47c-45df-908a-1e5bda46e918/image.png)

HTTP요청은 순차적으로 받아야함(1->2->3)

파이프라이닝을 통해 여러 요청을 많이 보내 순서에 맞게 응답을 받는 방식.

#### 파이프라이닝 치명적인 문제점

![img](https://media.vlpt.us/images/injoon2019/post/0543aded-eeb2-4cce-8c59-13050c9cf5f9/image.png)

첫요청이 오래걸리면 다른 요청들도 병목이 생김.

요청한 Request문제 발생, Latency증가.

=> Damain Sharding이 나옴.(도메인 명을 다르게해서 리소스를 나눠 저장함)=> 요청있으면 나눠진것에서 병렬적으로 리소스를 보내줌

but 이거도 대역폭을 너무 많이씀.

![img](https://media.vlpt.us/images/injoon2019/post/a1e8c9d5-0870-4aaf-b861-4da8de66a369/image.png)

중복이 많은데 그대로 전송을 함(주고받는 데이터가 쓸데없이 커짐.)

## HTTP2

![img](https://media.vlpt.us/images/injoon2019/post/6b23a348-f133-49b0-8678-b8bd1230743f/image.png)

대체가 아닌 기존의 확장.

![img](https://media.vlpt.us/images/injoon2019/post/e984cadf-18d2-4825-82df-b57239fac3b6/image.png)

컴퓨터가 좋아하는 바이너리로 전송함(텍스트가 아니라) => 파싱, 전송속도 올라가고 오류 발생 가능성 내려감.

메세지를 Header와 Data로 분할함.

![img](https://media.vlpt.us/images/injoon2019/post/63d83ce7-4158-4884-a764-739c2584ec85/image.png)

스트림이라는 데이터 양방향 흐름안에, 프레임 조각 조각들이 합쳐져서 요청이나 메시지가 된다.

![img](https://media.vlpt.us/images/injoon2019/post/eb3cc8c6-8a33-4e45-ae39-5095c71de292/image.png)

이걸 바탕으로 요청 응답이 다중화가 가능해짐. (프레임으로 쪼개져서 메시지간 순서가 사라짐)또 인터리븐이라고 끼어드는 방식도 있음.

=> Head of line blocking 해결

![img](https://media.vlpt.us/images/injoon2019/post/c9e1bd43-c21f-4b41-b371-e0e512c3a3ce/image.png)

리소스간 전송 우선순위 설정 가능(우선순위 가중치를 줘서 설정.)

![img](https://media.vlpt.us/images/injoon2019/post/97a7b070-c124-4d90-8e38-b2485b74f23f/image.png)

요청없어도 서버가 알아서 푸시해줌.(요청은 html이었으나 css, js도 알아서 푸시해줌.)

![img](https://media.vlpt.us/images/injoon2019/post/d777948b-9125-4a12-a459-d2ddcb9e48d3/image.png)

헤더 압축.(중복된 부분은 인덱스만 뽑고 중복 안된거는 허프만 인코딩을 해서 압축.)

![img](https://media.vlpt.us/images/injoon2019/post/1384d5cd-d71e-48a7-ad9b-23aff0163b53/image.png)

![img](https://media.vlpt.us/images/injoon2019/post/c82a8d35-c865-4264-87cb-fa80d32e4c80/image.png)



헤더를 85퍼센트 줄임. 페이지 로드시간도 줆.

## QUIC

![img](https://media.vlpt.us/images/injoon2019/post/7fb4f623-9004-4241-92fc-3d00f175d32c/image.png)

![img](https://media.vlpt.us/images/injoon2019/post/62998fdf-2d31-4c2e-9d3b-c72668946b2a/image.png)

UDP기반.

![img](https://media.vlpt.us/images/injoon2019/post/40bc43e1-f8a1-4486-a579-ed72aa39bc51/image.png)

TCP는 지연을 줄이기 힘듦.

![img](https://media.vlpt.us/images/injoon2019/post/cdecb4de-9523-40f7-b56d-495eefbb849f/image.png)

지연은 줄이고 신뢰성을 올리기위해 UDP기반으로 함.

![img](https://media.vlpt.us/images/injoon2019/post/26f4937a-e7c1-4cd4-9f57-9db80175b55d/image.png)

설정을 캐싱해 핸드셰이크 없이 다음연결때 전송을 가능하게함.

![img](https://media.vlpt.us/images/injoon2019/post/0a9bd989-c237-4214-90d4-3ce9e18ffcb9/image.png)

ip가 바뀌어도 유지됨

![img](https://media.vlpt.us/images/injoon2019/post/2c124d28-ec68-4f30-96a3-8a86d6d55675/image.png)

![img](https://media.vlpt.us/images/injoon2019/post/3806b0eb-9674-4ca3-92e9-076ded90aa65/image.png)

소스 어드레드 토큰을 필요에 따라 발급해 IP Spoofing, Replay Attack방지

![img](https://media.vlpt.us/images/injoon2019/post/3e129f1f-18af-4147-8b33-9c2d211743ad/image.png)

![img](https://media.vlpt.us/images/injoon2019/post/d69bfcb9-8d48-45d2-9cc1-0ffb5339b3eb/image.png)

TCP도 병목이 있을 수 있음(중간 데이터 손실 시) 녹색 망가져도 빨간색도 못움직임

![img](https://media.vlpt.us/images/injoon2019/post/0c8fe719-1b36-4e80-9121-e03551afb096/image.png)

노란색이 문제생기면 파란색은 정상적으로 수행

![img](https://media.vlpt.us/images/injoon2019/post/05a98f02-cf41-4faf-a624-22c8814a88ca/image.png)

HTTP3 이미나오고 구글은 쓰고있음. 아직 HTTP2가 점유율 40프로긴함.

![img](https://media.vlpt.us/images/injoon2019/post/40a94224-921b-4f4b-9c36-3e3dd0ebc8f9/image.png)

![img](https://media.vlpt.us/images/injoon2019/post/54ff3b58-0eb3-455b-9cce-0511a892b0e3/image.png)

## 네트워크 성능하락의 가장 큰 요인?

* Bandwidth
  * 데이터를 많이 주고받기 위해서는 큰 대역폭이 있으면 아니야?
  * 애초에 대역폭이 크면 데이터 전송 속도가 빨라지잖아?
  * => 실험 결과 한계가 있음. 대역폭은 일정 수준 이상부터 성능수준이 수렴함.
* Latency
  * 딜레이가 없어야지.

=> Latency가 성능의 핵심임.



---

HTTP의 발전은 TCP에 의해 이루어짐.(TCP성능 하락을 이겨내기 위해 HTTP가 발전을 함. TCP는 IP위에서 동작을 함.)

TCP는 느린전송을함(데이터 크기에 제한을 둠.) => 문제가 많음 => TCP 커넥션을 재활용할 방법을 찾기 시작함.(keep alive)

=> 1.1버전에서는 재화용이 가능하게 됨. but 병목은 해결 못함.

=>HTTP2 하나의 TCP커넥션을 통해 여러개를 병렬처리 할수있게 됨. but TCP HEAD OF LINE BLOCKING 발생.

=> 구글이 QUIC을 사용. => 독립 스트림(TCP체인을 병렬연결해 하나 병목생겨도 다른건 돌아감)을 통해 HEAD OF LINE 해결.



