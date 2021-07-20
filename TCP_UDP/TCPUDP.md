# TCP/UDP

## Transport Layer

### End Point 간 신뢰성있는 데이터 전송을 담당하는 계층

* 신뢰성 : 데이터를 순차적, 안정적인 전달
* 전송 : 포트 번호에 해당하는 프로세스에 데이터를 전송



---



### 전송계층이 없다면?

1. 데이터의 순차 전송 원활x
   * 송진자가 전달하고자했던 데이터가 안가게됨(1,2,3 -> 2,1,3)으로 바뀜.
2. Flow (흐름 문제)
   * 원인 : 송수신자 간의 데이터 처리 속도 차이 // 수신자가 처리할수있는 데이터량을 초과
3. Congestion (혼잡 문제)
   * 원인 : 네트워크의 데이터 처리 속도 (ex. 라우터) // Network 가 혼잡해서 데이터가 안오고 통신이안됨.



#### 결과 : 데이터의 손실 발생.



---



## TCP(Transmission Control Protocol)

* 신뢰성있는 데이터 통신을 가능하게 해주는 프로토콜
* 특징 : Connection 연결(3 way-handshake) - 양방향 통신

* 데이터 순차 전송을 보장

* Flow control (흐름제어)

* Congestion control (혼잡 제어)

* Error Detection (오류 감지)

  

---



### -세그먼트(Segment) - TCP 프로토콜의 PDU

#### 	데이터를 잘라 TCP Header를 붙힘. => 패키징한게 Segment





![img](https://blog.kakaocdn.net/dn/bCenX6/btqZP2MEqAq/PDyksDOf5nBnuY1et5Y2v0/img.png)

---





### TCP Header

![img](https://t1.daumcdn.net/cfile/tistory/215E874552342E7319)

* 순차전송

* Control Flags 필드(6bit)

  : 6개의 서로 다른 제어 비트 또는 플래그를 나타낸다. 동시에 여러 개의 비트가 1로 설정될 수 있다.

  

  \- **CWR : Congestion Window Reduced)** – 혼잡 윈도우 크기 감소

  \- **ECN : Explicit Congestion Notification)** – 혼잡을 알림

  \- **URG(Urgent)** : Urgent Pointer 필드가 가리키는 세그먼트 번호까지 긴급 데이터를 포함되어 있다는 것을 뜻한다.

   이 플래그가 설정되지 않았다면 Uregent Pointer 필드는 무시되어야 한다.

  \- **ACK(Acknowledgment)** : 확인 응답 메시지

  \- **PSH(Push)** : 데이터를 포함한다는 것을 뜻한다.

  \- **RST(Reset)** : 수신 거부를 하고자 할때 사용

  \- **SYN(Synchronize)** : 가상 회선이 처음 개설될 때 두 시스템의 TCP 소프트웨어는 의미 있는 확인 메시지를 전송하기 위해   일련 번호를서로 동기화해야 한다.

  \- **FIN(Finish)** : 작업이 끝나고 가상 회선을 종결하고자 할 때 사용

  

  ---

  

  ### 3 way handshake

  

![img](https://blog.kakaocdn.net/dn/bqWzBI/btqZV6NmcLk/KdR7yXGbKwWoH6b15jhLP0/img.png)

1. SYN 비트를 1로 설정해 패킷 송신

2. SYN. ACK 비트를 1로 설정해 패킷 송신

3.  ACK 비트를 1로 설정해 패킷 송신.

   => 1은 ok의 신호.(연결하자!)



---





### TCP통신방식

![img](https://blog.kakaocdn.net/dn/cxeJc9/btqZV5gCL1d/wbdeS5Stgr8nQ9mSfWdpEK/img.png)

1. client가 패킷 송신
2. server가 ACK송신
3. ACK를 수신하지 못하면 재전송(서버 응답 올때까지 )

=> 신뢰성 확보



---



#### 4 way -handchake(Connection close) 연결해제

1. 데이터를 전부 송신한 A가 FIN 비트를 송신(데이터 다 보냈다.)
2. B가 ACK 비트를 A로 송신(알았다.)
3. B에서 A로 남은 패킷 송신 ( 패킷 보낸다 기다려라 일정시간 대기)
4. B가 FIN 비트를 A로 송신(패킷 다 보냈다)
5. A가 ACK 비트를 B로 송신(알았다)
6. CLOSED (연결 해제됨)



---



### TCP의 문제점

**TCP의 문제점은 없을까?**



* 전송의 신뢰성은 보장하지만 다음과 같은 문제점이 있다.

- 매번 connection을 연결해서 시간적 손실이 발생한다. (3-way-handshaking)
- 패킷을 조금만 손실해도 재전송한다.



---



## UDP( **User Datagram Protocol**)



* TCP보다 신뢰성이 떨어지지만 **전송 속도가 TCP에 비해 빠른** Transport Layer의 protocol

  (순차전송x, 흐름제어x, 혼잡제어x)

- connectionless. 연결 요청-해제 과정이 필요 없다.(연결 과정 x일방적으로 보냄.)
- 에러 검출 기능만 있다.
- 비교적 데이터 신뢰성이 중요하지 않을 때 사용한다. (ex. 영상 스트리밍)

 ![img](https://blog.kakaocdn.net/dn/qj356/btqZTT815ff/t9bsCUehRXAVZbEQbr19DK/img.png)

* 7 Layer로 부터 받은 메세지를 세그먼트단위로 쪼개지 않고 UDP 헤더만 붙여 데이터그램단위로 처리. 단위(datagram) - 유저 데이터그램. 쪼개려면 직접 7계층에서 쪼개야함.

### **UDP Header**

- 출발지의 포트번호
- 목적지의 포트번호
- checksum: 오류 검출용 비트



### UDP의 데이터 전송방식

### ![img](https://blog.kakaocdn.net/dn/q0hsX/btqZYlXx6E9/gFN23XdAnKLhY4pATjCvk1/img.png)

- **Connectionless**: Client는 패킷을 확인 안하고 무조건 송신. Server는 소캣 무조건 열어두고 있음

### 특징

- 순차전송 ❌
- Flow Control ❌
- Congestion Control ❌
- Error detection: UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
- 비교적 데이터의 신뢰성이 중요하지 않을 때 사용(ex. 영상 스트리밍)



---



## TCP/UDP 비교

![img](https://blog.kakaocdn.net/dn/sv1cS/btqZTU1bcxs/ui7egay5SRnhTUOKjVy2v1/img.png)



---





## 결론!

1. TCP, UDP의 특성을 파악하고 상황에 따라 적절한 프로토콜을 사용할 수 있어야 한다.
2. TCP, UDP의 헤더에 대해 파악하고 성능 개선에 이용할 수 있어야 한다.







## 추가(TCP/IP 5계층 모델)

![img](https://blog.kakaocdn.net/dn/xe8Bq/btqZQHnXcry/zSWHllQe87UjrpAxGF9UP1/img.png)