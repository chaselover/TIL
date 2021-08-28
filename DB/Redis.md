디디님의 10분 테코톡 시청하며 작성했습니다.

# Redis

* Redis
* Cache
* Redis 자료구조
* Redis 주의사항

* Remote dictionary server(외부 해시맵 서버)



---

작년 쿠팡  redis 문제 -> key가 32bit CPI int범위를 넘어갔기 때문.

---

## What is Redis?

* Remote dictionary server
* Database, Cache, Message broker
* In-memory Data Structure Store
* Supports rich data structure



> DB는 HDD,SSD 메모리 계층에 존재해야함(꺼져도 손실되면 안됨)
>
> 하지만 Redis는 더 자주 접근하고 덜 자주 바뀌는 데이터를 더 빠른 Memory계층에 저장하기 위해
>
> In-memory층 즉 SRAM인 Cache층을 사용하는 DB이다.



* 파이썬의 List, Set, Dictionary, Sorted Set, Hash(object만들 저장하는 redis만의 자료구조)를 사용함.



## 자바의 자료구조를 안쓰고 redis를 쓰는이유

### 자바로 쓰면 안되는 이유

* 서버가 여러대인 경우 Consistency 문제 발생
  * 서버마다 다른 Data로 인해 불일치 문제 발생.
  * ex) session같은거 다른 server에서 알수없다
* 멀티 쓰레드에서 Race Conditon발생
  * 여러 쓰레드가 경합하는 것.
  * Context Switching에 따라 원하지 않는 결과가 발생

### Redis 쓰는 이유

* Race Condition 해결
  * Redis는 기본적으로 싱글 스레드기 때문에 Redis자료구조는 Atomic Critical Section(동시에 여러 프로세스가 접근하면 안되는 영역)에 대한 동기화를 제공합니다.
  * 즉 서로 다른 Transaction Read/Write를 동기화합니다.  



* 때문에 여러 서버에서 같은 data를 공유하거나 Atomic자료구조나 Cache를 쓰기위해서도 씀.



## 주의해야 할 점

* 싱글 쓰레드이므로 시간복잡도를 고려해야함
* In-memory 특성상 메모리 파편화, 가상 메모리등의 이해가 필요하다.



* Redis가 싱글쓰레드를 쓰는 이유.
  * Event Driven(비동기)
  * IO-bound Process(메모리 IO에서 보내는 시간이 길기 떄문에.CPU최적화의 효율이 크지 않기때문에 사용)
  * Context-Switching의 효율이 적다
  * Single Threaded
  * 사용의 단순함

* 네트워크의 요청을 받아서 명령을 수행
  * 빨리빨리 처리해야하고 그만큼 처리가 빨라야 한다. O(N)을 전부 뒤지는건 지양해야함



## Memory관리

* 메모리 파편화
  * 메모리의 파편들이 남아있으면 빈칸이 자리를 차지하게 됨
  * redis를 사용할때는 메모리를 넉넉히 잡아야함
* Virtual Memory - Swap
  * Main Memory와 Backing Store의 자주쓰는 Process여부에 대한 Swap을 지원. 
  * 하지만 latency가 발생하기 때문에 잘 고려해서 사용해야함.

* Replication - Fork
  * 복사해서 저장하는 방식을 사용하는데 메모리가 차면 에러가 나기 때문에 
  * 늘 메모리를 여유있게 잡아야한다.



## Key words

* Redis를 저장소 처럼 
  * Redis Persistent
  * RDB
  * AOF
* Redis의 메모리는 제한되어있기 때문에 주기적으로 Scale out, Back up을 해야함
  * Redis Cluster
* 부하분산
  * Constant Hashing
* Data Grid 
  * Spring Genfire, Hazlecast