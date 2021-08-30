# CPU Scheduling

## 스케줄링

* 실행준비가 된 프로세스 중에서 하나를 선택해 CPU를 할당 하는 것.

* 결국 CPU를 잘 사용하기 위해 프로세스를 잘 배정하기 위함.
  * 조건 : 오버헤드는 낮고, 사용률은 높고, 기아 현상은 낮을때.
  * 목표
    * Batch System : 가능하면 많은 일을 수행한다. 시간(time)보다 처리량(throughout)이 중요하다.
    * Interactive System : 빠른 응답 시간, 적은 대기 시간
    * Real-time System : 기한(deadline) 맞추기

## 선점 / 비선점 스케줄링

* 선점(preemptive) : OS가 CPU의 사용권을 선점 할 수 있는 경우, 강제 회수하는 경우
* 비선점(nonpreemptive) : 프로세스 종료 or I/O 등의 이벤트가 있을 때까지 실행 보장 (처리시간 예측 어려움.)

## 프로세스 상태에 따른 스케줄링 발생 상황

[![download (5)](https://user-images.githubusercontent.com/13609011/91695344-f2dfae80-eba8-11ea-9a9b-702192316170.jpeg)](https://user-images.githubusercontent.com/13609011/91695344-f2dfae80-eba8-11ea-9a9b-702192316170.jpeg)

- 비선점 스케줄링 발생 : 
  - 실행상태에서 준비상태로의 전환 : Interrupt
  - 준비상태에서 실행상태 변환시 : Scheduler Dispatch(단기 스케줄링)
- 선점 스케줄링 발생
  - 실행상태에서 대기상태로 전환 : I/O(입출력 요청) or Event Wait(대기 이벤트)
  - 대기상태에서 준비상태로 전환 : I/O(입출력 완료), Event Completion(완료 이벤트)
- Adimitted, Exit 상황에도 발생?

**프로세스의 상태 전이**

✓ **승인 (Admitted)** : 프로세스 생성이 가능하여 승인됨.

✓ **스케줄러 디스패치 (Scheduler Dispatch)** : 준비 상태에 있는 프로세스 중 하나를 선택하여 실행시키는 것.

✓ **인터럽트 (Interrupt)** : 예외, 입출력, 이벤트 등이 발생하여 현재 실행 중인 프로세스를 준비 상태로 바꾸고, 해당 작업을 먼저 처리하는 것.

✓ **입출력 또는 이벤트 대기 (I/O or Event wait)** : 실행 중인 프로세스가 입출력이나 이벤트를 처리해야 하는 경우, 입출력/이벤트가 모두 끝날 때까지 대기 상태로 만드는 것.

✓ **입출력 또는 이벤트 완료 (I/O or Event Completion)** : 입출력/이벤트가 끝난 프로세스를 준비 상태로 전환하여 스케줄러에 의해 선택될 수 있도록 만드는 것.

## CPU 스케줄링의 종류

- ### 비선점 스케줄링(Non-preemptive)

  1. #### FCFS (First Come First Served)

     - 큐에 도착한 순서대로 CPU 할당(FIFO)
     - 실행 시간이 짧은 게 뒤로 가면 평균 대기 시간이 길어짐
     - 일단 CPU를 잡으면 CPU burst가 완료될 때까지 CPU 를 반환하지 않는다. 할당 되었던 CPU가 반환될 떄만 스케줄링이 이루어진다.
     - 장점
       - 구현이 간단하다.
     - 문제점
       - convoy effect : 소요시간이 긴 프로세스가 먼저 도달하여 효율성을 낮추는 현상이 발생한다.(늦게온 빠른 작업에 불공평)
       - 평균 대기시간이 길어질 소요 생김.

  2. #### SJF (Shortest Job First)

     - 수행시간(CPU 점유시간)이 가장 짧다고 판단되는 작업을 먼저 수행(CPU에 먼저 할당)
     - FCFS 보다 평균 대기 시간 감소, 짧은 작업에 유리
     - 문제점
       - 요구시간이 긴 프로세스가 요구 시간이 짧은 프로세스에 항상 양보되어 기아 상태(starvation) 발생 가능.
     - SRTF(shortest-Remaining-time-first)
       - 새로운 프로세스가 도착할때마다 스케줄링을 세로한다.
       - SJF의 선점 스케줄링 기법
         * 장점 : 최적화, 주어진 프로세스 집합에 대해 최소 평균 대기시간을 제공.
         * 단점 : 예측이 힘듦(스케줄링을 매번 다시하기 때문에 CPU burst time 측정 불가), 불공정함(기아).

  3. #### HRN (Hightest Response-ratio Next)

     - 우선순위를 계산하여 점유 불평등(긴 작업과 짧은 작업의 불평등)을 보완한 방법(SJF의 단점 보완)
     - 우선순위 = (대기시간 + 실행시간) / (실행시간)
     - 서비스 받는 시간(run time)이 분모에 있으므로 짧은 작업의 우선순위가 높아진다.
     - 대기시간waiting time이 분자에 있으므로 긴작업도 대기시간이 커지면 우선순위가 높아진다.

- ### 선점 스케줄링

  1. #### Priority Scheduling

     - 정적/동적으로 우선순위를 부여하여 우선순위가 높은 순서대로 처리
     - 더 높은 우선순위의 프로세스가 도착하면 Ready Queue의 Head에 넣는다.
     - 문제점
       - 우선 순위가 낮은 프로세스가 무한정 기다리는 Starvation 이 생길 수 있음
       - 준비는 되어있으나 CPU를 사용못하는 프로세스를 CPU가 무기한 대기하는 상태인 무기한 봉쇄(Indefinite blocking)
     - 해결책
       - Aging 방법으로 Starvation 문제 해결 가능 : 시간이 지날수록 우선순위가 낮은 프로세스의 우선순위를 높이는 방법.

  2. #### Round Robin

     * 현대적인 기법.

     * 프로세스가 ready queue를 순환하듯 돌면서 할당받는 기법.
       * 프로세스는 순서대로 CPU의 시간 단위만큼 할당받음(선점 기법)
       * 이 시간이 경과하면 프로세스는 레디큐의 끝에 추가되고 다음 프로세스가 할당받음.
     * 장점
       * 어떤 프로세스도 (n-1)q시간 단위를 기다리지 않게 됨.(기다리는 시간이 CPU 쓸만큼 증가 공정한 스케줄링)

     - 단점
       - 할당 시간(Time Quantum)이 크면 FCFS(FIFO)와 같게 되고, 작으면 쓰레드 간 문맥 교환 (Context Switching) 잦아져서 오버헤드 증가(시간 소모 커짐.)
     - FCFS에 의해 프로세스들이 보내지면 각 프로세스는 동일한 시간의 Time Quantum만큼 CPU를 할당 받음
       * Time Quantum = Time Slice = 실행의 최소 단위 시간
     - CPU사용시간이 랜덤한 프로세스들이 섞여있을 경우 효율적.
     - 프로세스의 context를 save할 수 있기 때문에 가능함.

  3. #### Multilevel-Queue (다단계 큐)(다중레벨 피드백 큐)

     [![Untitled1](https://user-images.githubusercontent.com/13609011/91695428-16a2f480-eba9-11ea-8d91-17d22bab01e5.png)](https://user-images.githubusercontent.com/13609011/91695428-16a2f480-eba9-11ea-8d91-17d22bab01e5.png)

     - 작업(ready queue)들을 여러 종류의 그룹으로 나누어 여러 개의 큐를 이용하는 기법 / 나누어 우선순위를 부여
     - [![Untitled](https://user-images.githubusercontent.com/13609011/91695480-2a4e5b00-eba9-11ea-8dbf-390bf0a73c10.png)](https://user-images.githubusercontent.com/13609011/91695480-2a4e5b00-eba9-11ea-8dbf-390bf0a73c10.png)
     - 우선순위가 낮은 큐들이 실행 못하는 걸 방지하고자 각 큐마다 다른 `Time Quantum`을 설정 해주는 방식 사용
     - 우선순위가 높은 큐는 작은 `Time Quantum` 할당. 우선순위가 낮은 큐는 큰 `Time Quantum` 할당.

  4. #### Multilevel-Feedback-Queue (다단계 피드백 큐)

     [![Untitled2](https://user-images.githubusercontent.com/13609011/91695489-2cb0b500-eba9-11ea-8578-6602fee742ed.png)](https://user-images.githubusercontent.com/13609011/91695489-2cb0b500-eba9-11ea-8578-6602fee742ed.png)

     - 다단계 큐에서 자신의

        

       ```
       Time Quantum
       ```

       을 다 채운 프로세스는 밑으로 내려가고 자신의

        

       ```
       Time Quantum
       ```

       을 다 채우지 못한 프로세스는 원래 큐 그대로

       - Time Quantum을 다 채운 프로세스는 CPU burst 프로세스로 판단하기 때문

     - 짧은 작업에 유리, 입출력 위주(Interrupt가 잦은) 작업에 우선권을 줌

     - 처리 시간이 짧은 프로세스를 먼저 처리하기 때문에 Turnaround 평균 시간을 줄여준다.

  5. 다중 레벨 피드백 큐 구현시 고려사항

     * 대기열 수

     * 각 큐에 대한 스케쥴 알고리즘

     * 프로세스를 업그레이드 할 시기를 결정하는 데 사용되는 함수

     * 프로세스를 강등할 시기를 결정한느데 사용되는 함수

     * 프로세스가 서비스를 필요로 할 때 프로세스가 입력할 큐를 결정하는데 사용되는 함수.

     * ```bash
       Multilevel Feedback Queue의 예
       Q1. RR (time quantum = 8)
       Q2. RR (time quantum = 16)
       Q3. FCFS
       ```

       

## CPU 스케줄링 척도

1. Response Time
   - 작업이 처음 실행되기까지 걸린 시간
2. Turnaround Time
   - 실행 시간과 대기 시간을 모두 합한 시간으로 작업이 완료될 때 까지 걸린 시간

------

### 출처

* https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/CPU%20Scheduling.md

- https://github.com/jobhope/TechnicalNote/blob/master/operating_system/CPUScheduling.md
- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#%EB%A9%80%ED%8B%B0-%EC%8A%A4%EB%A0%88%EB%93%9C

