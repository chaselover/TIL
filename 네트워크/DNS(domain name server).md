# DNS(domain name server)

* Domain name을 ip주소로 바꿔주는 시스템(서비스)

* IP로 통신하던 방식 

  -> hosts파일에 domain name과 IP주소를 적어놓고 대응하는 방식 

  -> SRI-NIC에서 전세계의 IP와 도메인을 수집해서 유저는 그곳을 통해서 hosts파일을 받도록 바뀜(개인이 hosts파일을 관리하기에는 너무 소요가 많아서.)

  ->DNS가 고안됨.

  

> IP주소 없이 도메인 네임을 통해 우리는 원하는 웹 사이트로 갈 수 있음.
>
> * 쿼리를 통한 매핑.





## DNS통신 구성 요소

다른 서버들과 메세지를 통해 소통하기 위한?

DNS 서버에 질의 dev.taggle.kr A IN

(Query name string : 질의를 하고싶은 대상. / Type : 어떤 데이터를 받고싶은지 (A는adress,NS는 name server)/ Class : DNS프로토콜이 어떤 환경에서 통신하고있나? 근데 현재는 internet밖에 없어서 거의 IN고정값임.)

DNS 서버의 응답 dev.taggle.kr A IN(data section) -> 가지고있는 IP Adress를 찾아 반환한다.(resource section)



>  ++DNS 리소스 레코드(서버가 가지고잇는 리소소의 모음)



## DNS 서버의 종류

* 권한없는 네임 서버(non-Authoritative Name Server)
* 권한있는 네임 서버(Authoritative Name Server)



### 권한 없는 서버 - Public DNS(재귀 DNS 확인자.recursive resolver)

통신사들(인터넷 서비스 제공업체(ISP)이 자체적으로 서비스하는 서버. 자기 스스로 정보가 없기때문에 다른 객체한테 시킴.

최전방에서 client에서 요청을 직접 받고 응답을 내려줄 수 있다. 하지만 권한이 없기때문에 다른 서버들에 물어봐서 해결해준다.



> 도메인 네임.
>
> Root-domain
>
> Top Level Domain
>
> Second level Domain
>
> Third level Domain(sub Domain)



![how-route-53-routes-traffic](https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png)

>  dev.taggle.kr A IN 요청을 계층적으로 권한있는 서버에 문의해(root부터kr.com등등..) client에게 반환함.



### 권한 있는 서버 - 계층형 Name Server

Root Name Server

Top level Domain Server(.kr서버.com서버)

Other Authoritative Name Server(hosting.kr(IP주소 있는 서버))(권한있는 이름서버.)



> 권한있는 서버는 권한없는서버의 요청에 따라 응답해줌.





---

### DNS 질의 절차.

1. 사용자가 웹 브라우저에 'example.com'을 입력하면, 쿼리가 인터넷으로 이동하고 DNS 재귀 확인자가 이를 수신합니다.
2. 이어서 확인자가 DNS 루트 이름 서버(.)를 쿼리합니다.
3. 다음으로, 루트 서버가, 도메인에 대한 정보를 저장하는 최상위 도메인(TLD) DNS 서버(예: .com 또는 .net)의 주소로 확인자에 응답합니다. example.com을 검색할 경우의 요청은 .com TLD를 가리킵니다.
4. 이제, 확인자가 .com TLD에 요청합니다.
5. 이어서, TLD 서버가 도메인 이름 서버(example.com)의 IP 주소로 응답합니다.
6. 마지막으로, 재귀 확인자가 도메인의 이름 서버로 쿼리를 보냅니다.
7. 이제, example.com의 IP 주소가 이름 서버에서 확인자에게 반환됩니다.
8. 이어서, DNS 확인자가, 처음 요청한 도메인의 IP 주소로 웹 브라우저에 응답합니다.
9. DNS 조회의 8단계를 거쳐 example.com의 IP 주소가 반환되면, 이제 브라우저가 웹 페이지를 요청할 수 있습니다.

10. 브라우저가 IP 주소로 [HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/) 요청을 보냅니다.
11. 해당 IP의 서버가 브라우저에서 렌더링할 웹 페이지를 반환합니다(10단계).



1. 만약 라우팅 중 프록시 서버를 만난다면 웹 캐시에 저장된 정보를 response 받는다.
2. 프록시 서버를 만나지 못해 www.test.com을 서빙하는 서버까지 간다면 서버에서 요청에 맞는 데이터를 response로 전송한다.
3. 브라우져의 loader가 해당 response를 다운로드할지 말지 결정을한다.
4. 브라우져의 웹 엔진이 다운로드한 .html 파일을 파싱하여 DOM 트리를 결정한다.
5. .html 파싱중 script 태그를 만나면 파싱을 중단하는 것이 원칙(지연 가능).
6. script 태그에 있는 자원을 다운로드 하여 처리가 완료되면 다시 파싱을 재개한다.
7. CSS parser가 역시 .css 파일을 파싱하여 스타일 규칙을 DOM 트리에 추가하여 렌더 트리를 만든다.
8. 이 렌더트리를 기반으로 브라우져의 크기에 따라 각 노드들의 크기를 결정한다.
9. 페인트한다 : 렌더링 엔진이 배치를 시작한다.



---

### DNS 쿼리 3가지 유형

#### 세 가지 유형의 DNS 쿼리:

1. **재귀 쿼리** - 재귀 쿼리에서는, 확인자가 레코드를 찾을 수 없는 경우, DNS 클라이언트는 DNS 서버(일반적으로 DNS 재귀 확인자)가, 요청한 자원 레코드 또는 오류 메시지를 사용하여 클라이언트에 응답하도록 요구합니다.
2. **반복 쿼리** - 이 경우, DNS 클라이언트는 DNS 서버가 가능한 최상의 응답을 반환하도록 합니다. 쿼리한 DNS 서버가 쿼리 이름과 일치하는 이름을 갖고 있지 않은 경우, 하위 수준의 도메인 네임스페이스에 대해 권한 있는 DNS 서버에 대한 참조를 반환합니다. 그러면 DNS 클라이언트가 참조 주소를 쿼리합니다. 이 프로세스는 오류 또는 제한 시간 초과가 발생할 때까지 추가 DNS 서버가 쿼리 체인을 중단한 상태로 계속됩니다.
3. **비재귀 쿼리** - 일반적으로, DNS 확인자 클라이언트의 쿼리를 받은 DNS 서버가 해당 레코드에 대한 권한이 있거나 캐시 내부에 해당 레코드를 갖고 있어, DNS 서버가 액세스 권한을 갖고 있는 레코드를 쿼리할 때 발생합니다. 일반적으로, DNS 서버는 추가 대역폭 소비 및 업스트림 서버의 부하를 방지하기 위해 DNS 레코드를 캐시합니다.



---





## DNS 캐싱이란 무엇입니까? DNS 캐싱은 어디서 발생합니까?

캐싱의 목적은 데이터를 임시 저장하여, 데이터 요청에 대해 성능과 신뢰성을 높이는 것입니다. DNS 캐싱은 요청하는 클라이언트와 가까운 곳에 데이터를 저장함으로써, DNS 쿼리를 조기에 확인할 수 있고 DNS 조회 체인의 추가 쿼리를 피할 수 있으므로, 로드 시간이 향상되고 대역폭/CPU 소비가 줄어듭니다. DNS 데이터는 다양한 위치에 캐시될 수 있으며, 각 위치는 [TTL(Time-To-Live)](https://www.cloudflare.com/learning/cdn/glossary/time-to-live-ttl/)에 의해 정의된 설정 시간 동안 DNS 레코드를 저장합니다.

#### 브라우저 DNS 캐싱

최신 웹 브라우저는 기본적으로 정해진 시간 동안 DNS 레코드를 캐시하도록 설계되었습니다. 그 목적은 분명합니다. DNS 캐싱이 웹 브라우저와 가까울수록, 캐시를 확인하고 IP 주소에 대한 올바른 요청을 하기 위해 처리해야 할 단계가 적어집니다. DNS 레코드를 요청할 때, 브라우저 캐시에서 처음으로, 요청한 레코드를 확인하는 것입니다.

Chrome에서는 chrome://net-internals/#dns에서 DNS 캐시의 상태를 볼 수 있습니다.

#### 운영 체제(OS) 수준 DNS 캐싱

운영 체제 수준 DNS 확인자는 DNS 쿼리가 컴퓨터를 떠나기 전의 두 번째 중단점이며, 로컬에 있는 마지막 중단점입니다. 이 쿼리를 처리하도록 설계된 운영 체제 내부의 프로세스를 일반적으로 "스텁 확인자" 또는 DNS 클라이언트라고 합니다. 스텁 확인자는 애플리케이션에서 요청을 받으면 먼저 자체 캐시를 검사하여 레코드가 있는지 확인합니다. 레코드가 없으면 로컬 네트워크 외부의 (재귀 플래그가 설정된) DNS 쿼리를 인터넷 서비스 공급자(ISP) 내부의 DNS 재귀 확인자로 보냅니다.

ISP 내부의 재귀 확인자가 모든 이전 단계와 같이 DNS 쿼리를 수신하면, 요청한 호스트-IP-주소 변환이 로컬 지속성 계층 내에 이미 저장되어 있는지도 확인합니다.

재귀 확인자에는 캐시에 있는 레코드 유형에 따른 추가 기능도 있습니다.

1. 확인자가 [A 레코드](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/)는 갖고 있지 않지만, 권한있는 이름 서버에 대한 [NS 레코드](https://www.cloudflare.com/learning/dns/dns-records/dns-ns-record/)를 갖고 있는 경우에는, DNS 쿼리의 여러 단계를 거치지 않고 해당 이름 서버를 직접 쿼리합니다. 이 바로가기는 루트 및 .com 이름 서버(예: example.com 검색)로부터의 조회를 방지하고 DNS 쿼리의 확인이 더 빨리 이루어지도록 도와줍니다.
2. 확인자에 NS 레코드가 없는 경우, 루트 서버를 건너뛰고 TLD 서버(이 경우 .com)로 쿼리를 보냅니다.
3. 확인자에 TLD 서버를 가리키는 레코드가 없는 경우, 루트 서버를 쿼리합니다. 이 이벤트는 일반적으로 DNS 캐시가 제거된 후에 발생합니다.



