

멍토님, 우의님의 10분 테코톡 보고 작성했습니다.



# Blocking vs Non-Blocking, Sync vs Async



* 제어권(행동할수있는 권리, 행동할수 있는 시간)의 반환
* 결과값의 전달 (return)



## 1. Blocking vs Non-Blocking

- 제어할 수 없는 대상의 처리 방법.

![img](https://blog.kakaocdn.net/dn/bELduX/btq6LWjvbi6/mr44ZDH6vWHcnigUzkC1k0/img.png)

 -Blocking : 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 **다른 작업이 끝날 때까지 **기다렸다가 자신의 작업을 시작하는 것

- 호출된 함수가 자신의 작업을 모두 마칠 때까지 호출한 함수에게 제어권을 넘겨주지 않고 대기하게 만든다
- 호출되는 함수가 바로 리턴하지 않으면 Blocking



>  호출한 함수가 제어권을 가지고있따가 function A에게 가서 제어권을 줌, 
>
> 함수 시행 후 제어권에 결과값을 얹어서 호출한 메인함수에게 줌.



### -Non - Blocking : 다른 주체의 작업에 **관련없이** 자신의 작업을 하는 것.

![img](https://blog.kakaocdn.net/dn/dl7ZrM/btq6CNVTAv6/HtvbF2iOWnj3cm8L87cYOk/img.png)

 Non-Blocking은 A의 작업 도중 B를 호출한 후 B의 작업과 상관없이 A의 나머지 작업을 수행하는 것이다. 

이 경우 제어권은 B에게 넘어가도 B는 즉시 결과를 return하여 A에게 제어권을 돌려준다. 

=> 호출된 함수가 바로 return해서 호출한 함수에게 제어권을 넘겨주고 호출한 함수가 다른 일을 할 수 있는 기회를 준다.

* 호출되는 함수가 바로 return하면 Non-Blocking



> 얘는 제어권을 호출되는 함수에게 주려고 하나 다시 바로 넘어옴. 
>
> 호출되는 함수의 결과값은 호출한 함수로 가는지 어디로 가는지 아무도 모름.



---



# 2. Synchronous vs Asynchronous

 

결과를 돌려 주었을 때 순서와 결과(처리)에 관심이 있는지 아닌지로 구분.

작업을 수행하는 두 주체가 서로의 작업의 시작/완료 여부를 확인해야 하는가에 대한 개념이다. 

더 정확히는 작업을 요청한 측(A)에서 요청한 작업(B)의 완료여부를 확인하면 동기, 그렇지 않으면 비동기이다. 

동기 : 데이터를 같은 시간에 같이 맞춰주는 것(시간이 일치하는가?)(어떤 대상? 어떤 시간?)



### -Synchronous



![img](https://blog.kakaocdn.net/dn/GutGz/btq6CAQb2KH/5bpHurTWZiWh68F4GqEqBK/img.png)





 동기 작업은 다수의 작업의 주체들이 서로 동시에 시작하거나(B,C), 끝나거나(B,C) 혹은 끝나는 동시에 시작(A->C)하는 경우이다.

 A함수에서 B함수를 호출했을 때 B함수의 완료여부를 A가 기다리거나 B의 작업 완료 여부를 계속 확인한다면 동기이다.

**끝나는 동시에 시작한다**. - 결과와 순서에 관심이 많다.(제어권 반환과 결과값 전달이 동시? 블락과 비슷. 같을때도 있고.)

그냥 관점의 차이다. 블록은 제어권에 대한 이야기, 동기는 제어권을 반환하는 시간에 대한 이야기.



### -Asynchronous

![img](https://blog.kakaocdn.net/dn/Q0Fb5/btq6Mau6Hgf/pN00LMek58T0H5zKxOwPeK/img.png)



 비동기 작업은 다수의 작업의 주체들이 서로의 작업의 시작/완료 시간과 상관없이 별도로 작업을 시작하고 종료하는 경우이다. 

예를 들어 A가 B함수를 호출하면서 콜백함수를 함께 전달하여 B의 작업 완료와 상관없이 자신의 작업을 계속해나가는 것이다.

돌아온 결과에 대해서도 결과를 처리할수도 안할수도 있음.(그냥 가던 안가던 기다리던 신경안씀.)



---



## 3. 조합

![img](https://blog.kakaocdn.net/dn/maJfY/btq6EVe7vtF/AmViFYnVCqBYj7yCI0T8Ak/img.jpg)



### 1. Sync Blocking

![img](https://media.vlpt.us/images/guswns3371/post/70c4e034-7c0a-497b-9aea-b679ca54aef1/image.png)

  동기 블로킹은 작업들의 시작 시간, 종료 시간이 서로 영향을 받으면서 + 다른 작업을 하는 동안 자신의 작업을 중지해야하는 경우이다. 대부분의 함수 호출이 이에 속한다.



**Blocking** 👉 다른 작업이 시작하게되면, 원래의 작업을 멈춘다
**Sync** 👉 결과를 반환받으면, 바로 처리한다



ex. 자바에서 입력요청시 제어권이 입력으로 넘어간다. 그래서 입력을 받기 전까지 다음 코드가 실행되지 않고, 입력을 받으면 제어권과 함께 결과를 같이 받아 처리한다.



### 2. Sync Non-blocking

non블록시 결과값이 어디로 가는가? Application -> kernel(안끝났는데 바로 반환(결과값없이.))

(완료된 상태만 결과값이 아니라 ''완료되지 않음'이라는 상태도 결과값이기 때문에 이걸 반환함.')

결과값이 완료되면 바로 처리한다.

컨텍스트 스위칭이 계속일어남(비용이 발생함.)

![img](https://media.vlpt.us/images/guswns3371/post/e7f9715b-a3e9-424a-83f8-684701086aee/image.png)

 동기 논블로킹은 작업들의 시작 시간, 종료시간이 서로 영향을 받으면서 + 다른 작업이 끝나기를 기다리지 않는 경우이다. A작업이 자신의 작업을 수행하면서 B작업이 완료여부를 지속적으로 확인하는 경우가 이에 속한다. Java의 Future.isDone()이 이에 속한다고 한다. 



**Non-Blocking** 👉 다른 작업이 시작하게되어도, 원래의 작업에 대한 제어권을 가지고 진행한다
**Sync** 👉 결과를 반환받으면, 바로 처리한다



ex. 게임에서 맵 데이터를 가져올 때, 유저에게 정보 로드율을 보여주는 경우. (progress )



### 3. Async Blocking(Java)

![img](https://media.vlpt.us/images/guswns3371/post/3e7e01e5-bd0c-404e-a4fa-ac3dcbbb198f/image.png)

  비동기 논블로킹은 작업들이 서로의 시작/종료 시간에 관심이 없고 + 다른 작업을 하는 동안 자신의 작업을 중지해야하는 경우이다. 예를 들면 Node.js에서 MySQL을 쓸 경우이다. Node.js에서 MySQL호출 시 MySQL의 드라이버를 사용하는데 이 드라이버가 Blocking방식이라 Node.js의 나머지 코드는 동작을 할 수 없다고 한다.([관련블로그글](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jungwan82&logNo=221173406494)) 



**Blocking** 👉 다른 작업이 시작하게되면, 원래의 작업을 멈춘다
**Async** 👉 결과를 반환받아도, 바로 처리하지 않을 수 있다.



보통 개발자의 실수로 Non-Blocking/Async 형식으로 동작하려던 작업이 의도하지 않게 Blocking/Async 으로 동작하는 경우가 있다. NonBlocking/Async 방식을 쓰는데 그 과정 중에 하나라도 Blocking으로 동작하는 작업이 포함되어 있다면 의도하지 않게 Blocking/Async로 동작할 수 있다.



###  4. Async Non-Blocking(Javascript)

![img](https://media.vlpt.us/images/guswns3371/post/d4101e32-e416-44c7-b143-753e705c6bf6/image.png)

 비동기 논블로킹은 작업들이 서로의 시작/종료 시간에 관심이 없고 + 다른 작업이 끝나기를 기다리지 않는 경우이다. 예를 들어 비동기 함수를 호출하며 콜백함수를 함께 전달하는 경우가 이에 속한다.

 

**Non-Blocking** 👉 다른 작업이 시작하게되어도, 원래의 작업에 대한 제어권을 가지고 진행한다
**Async** 👉 결과를 반환받아도, 바로 처리하지 않을 수 있다.
**성능과 자원의 효율적 사용 관점에서 가장 유리한 모델은 Non-Blocking/Async 모델이다.**



ex. 자바스크립트에서 API 요청을 한뒤 다른 작업을 수행하다가 callback 함수를 통해 추가적 작업을 처리할 때 사용된다.





---



## 결론



BLocking vs non Blocking -> 제어의 관점 => 제어할 수 없는 대상을 어떻게 처리하는가?

제어권은 누가가지고 언제 반환할꺼냐?

sync vs Async -> 순서와 결과(처리)의 관점. => 제어권 반환, 결과값 전달, 시작하는 시간 즉 대상들의 시간을 어떻게 일치시키느냐의 개념.

![image-20210722204901673](C:\Users\한승주\AppData\Roaming\Typora\typora-user-images\image-20210722204901673.png)

실제로는 이따구로 뭔가 여기저기 맞는 처리방법이 다 달라서 딱 잘라서 어떤 프로그래밍이다 말하기 어려운 개념인 것 같다. 

방식을 이해하고 적재적소에 사용할 수 있게끔 공부하자.