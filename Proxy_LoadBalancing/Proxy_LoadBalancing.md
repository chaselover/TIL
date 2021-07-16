---

# Proxy(Network Proxy)

## Proxy Server

* 클라이언트와 서버간의 중계 서버로, 통신을 대리 수행하는 서버
* http통신을 중계

* 캐시/보안/트래픽 분산 등 여러 장점을 가질 수 있음.

  ---

  

## Forward Proxy

* Proxy server설정을 한다..
* 인터넷 속도 향상을 위해 윈도우 Proxy설정을 해줘라.
* 외국에서 접속하는 것 처럼 하기 위해, IP추적 방지를 위해 Proxy를 써라



=> 클라이언트와 인터넷 사이에 위치를 하고있고 Intenet대신 Forward Proxy가 흐름처리를 하고있다.

---



### 특징

#### 캐싱

1. 전송 시간 절약( 특히 고속 인터넷 사용자 )

2. 불필요한 외부전송 X

3. 외부 요청 감소

   -> 네트워크 병목 현상 방지(인터넷 대역폭 비용을 줄이기 위해.)



#### 익명성

-우리가 요청했지만 마치 Forward Proxy가 요청한 것 처럼

**Server가 응답 받은 request를 누가 보냈는지 알지 못하게 함.**

(서버는 Proxy IP를 받게 됨.)(필터, 접근제어(권한에 따라 PW요구))



### TransCoder

-때로는 데이터를 조작할 수도 있음.(데이터압축, 언어 변환.)



---



## Reverse Proxy

인터넷과 서버들 사이에 위치.

#### 캐싱

#### 보안

-서버 정보를 클라이언트에게 숨김.(Reverse Proxy를 실제 서버로 생각하게 함.)



## Load Balancing

: 부하분산 : 해야 할 작업을 나눠서 서버의 부하를 분산시키는 서비스.

-request를 각각 서버들이 원하는 대로 나눠주는 것.(다양한 알고리즘이 존재(궁금하면 공부))



? 서비스 트래픽 증가로 인해 하나의 서버로 감당이 불가능

=> Scale UP!(서버 성능을 올리자!)

? 그래도 안 돼?

=> Scale Out!(여러대의 서버로 나누자!)

미리 측정된 웹 서버의 능력에 따라 분배 or 서버의 부하상태에 따라 분배.



종류는 OSI 7 Layer 기준으로 L2, L3, **L4, L7**을 기준으로 나눔.

(L2는맥주소, L3은 IP주소)

### L4(Transport Layer)

* IP & Port Level에서 로드 밸런싱(TCP/UDP)

* 그냥 포트 80으로 오는 트래픽을 서버A, B로 나눠줌. 



### L7(Application Layer)

* User Request Level에서 로드 밸런싱(HTPPS/HTTP/FTP)
* URL, Query, Route에 따라 로드 밸런싱.