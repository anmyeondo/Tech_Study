# 데드락(DeadLock)

- 시스템 자원에 대한 요구가 뒤엉켜 두 개 이상의 작업이 서로 끝나기를 기다리며 어느것도 종료되지 못하고 교착상태에 빠져 있는 것을 의미

- 프로세스1과 2가 자원1, 2를 모두 얻어야 한다고 가정해보자

  - t1 : 프로세스1이 자원1을 얻음 / 프로세스2가 자원2를 얻음

  - t2 : 프로세스1은 자원2를 기다림 / 프로세스2는 자원1을 기다림

- 현재 서로 원하는 자원이 상대방에 할당되어 있어서 두 프로세스는 무한정 wait 상태에 빠짐 -> 이 상태를 DeadLock이라고 함

## 데드락의 발생조건
1. Mutual exclusion (상호 배제): 공유가 불가능한 자원이 적어도 하나 존재
2. Hold and Wait (점유 대기): 어떤 자원을 획득한 상태로 다른 자원을 기다림
3. No preemption (비선점): 더 이상 진행되지 않음에도 불구하고 가지고 있는 자원을 놓지 않음
4. Circular Wait (순환 대기): 일련의 그룹에 Cycle이 발생

## Resource-Allocation Graph
- 시스템의 자원 할당 상황을 그래프로 그린 것으로 Deadlock을 판단하기 위해 만들어짐
- P: Process, R: Resource, R 내부의 동그라미: Instance
- P -> R: Request Edge - 프로세스가 해당 자원을 요청
- R -> P: Assignment Edge - 해당 자원이 프로세스에 할당되어 있음
- single instance: cycle <-> deadlock
- multiple instance: deadlock -> cycle | !cycle -> !deadlock

<img width="281" alt="deadlock-2" src="https://user-images.githubusercontent.com/29935109/210229304-4c637705-8a5e-4582-b74b-9bc73a4e9973.png"><img width="302" alt="deadlock-3" src="https://user-images.githubusercontent.com/29935109/210229314-04268bf3-6b8a-4b76-87ba-c5dd63f052a0.png">

## 데드락의 해결 방법
### Deadlock prevention (예방)
- 필요조건 4개 중 적어도 하나 이상 성립이 안되도록 하여 데드락을 방지하는 방법
 1. Mutual exclusion
    - 공유 자원이 너무 많아서 이 조건을 막기는 어려움
    - 한 번에 여러 프로세스가 공유 자원을 사용할 수 있도록 변경할 수는 있으나 동기화 문제가 발생할 수도 있음
 2. Hold and Wait
    - 프로세스를 시작하기 전에 필요한 자원을 한꺼번에 할당하거나 잡고 있는 자원이 없을 때만 자원을 요청함
    - Low resource utilization과 starvation의 문제가 발생할 수 있음
 3. No Preemption
    - 새로운 자원 할당이 불가능하면 우선순위가 낮은 프로스세가 가지고 있는 자원을 다 내놓거나 대기 중인 프로세스의 자원을 반납하게 함
    - 자원을 다 할당받을 수 있을 때 프로세스를 다시 시작함
 4. Circular Wait
    - total ordering을 통해 모든 프로세스에 우선순위를 정하고 어떤 자원을 요청할 때 그 자원보다 높은 순서의 자원을 가질 수 없음 (P5 -> P0 -> P1 -> P5에서 P5 -> P0를 불가능하게 만듦)
- 1~3번 조건은 구현하기 굉장히 까다롭기 때문에 4번을 통해서 prevention 하는 것이 가장 현실적인 방법

### Deadlock avoidance (회피)
 - 필요한 자원에 대한 정보를 이용하여 Deadlock이 일어나지 않는 방향으로 프로세스를 진행
 - resource-allocation state가 circular-wait condition이 되지 않도록 하는 알고리즘을 일컬음
 - Safe State
   - 시스템에 있는 모든 프로세스가 각자 필요한 자원을 사용할 수 있는 실행 순서가 존재할 때 시스템이 'Safe State'에 있다고 말함
   - Safe Sequence: 사용가능한 자원을 특정 process에게 주고 그 프로세스가 끝난 후 해당 자원을 받은 모든 프로세스의 요구를 처리해줄 수 있는 순서를 의미
 - Basic Facts
   - safe state -> no deadlocks
   - unsafe state -> deadlock 가능성 있음
   - avoidance는 시스템이 unsafe state로 들어가지 않는 것을 보장하는 것
   <img width=40% src="https://user-images.githubusercontent.com/29935109/210230008-94f95356-73b6-469b-8f56-fe9dbb4b58d6.jpeg">
 - Avoidance algorithm
   - resource-allocation graph(single instance)
     <p align="left"><img src="https://user-images.githubusercontent.com/29935109/212645827-cdc7c822-b5b8-4432-9f15-6b93fdff1222.jpg"></p>
     - resource-allocation graph를 사용해 process의 수행 순서를 결정함
     - 미래에 사용할 자원을 나타내는 Claim Edge를 사용(점선)<br/>
        -> 요청할 때 점섬이 실선으로 바뀜. 이때 cycle이 생기지 않으면 자원을 할당
   - Banker's algorithm(multiple instance)
     - 가진 자원 정보를 가지고 safe state가 존재하는지 확인하여 deadlock을 회피할 수 있게 하는 알고리즘
     - 프로세스가 사용할 자원들을 미리 보고 safe sequence를 찾는 알고리즘
     <p align="left"><img src="https://user-images.githubusercontent.com/29935109/212644913-0e7d63a4-8697-4bd8-9213-fda75af321eb.jpg"></p>
     - 사용되는 변수
       1. Available: 각 Resource 별로 할당할 수 있는 남은 인스턴스 수
       2. Max: Process가 하나의 타입의 resource에 최대로 요구하는 인스턴스 수
       3. Allocation: Process에 할당된 resource의 인스턴스 수
       4. Need: Process가 필요로 하는 resource의 인스턴스 수
     - 단점
       1. 할당할 수 있는 자원의 수가 일정해야 함
       2. 사용자 수가 일정해야 함
       3. 항상 불안전 상태를 방지해야 하므로 자원 이용도가 낮음
       4. 최대 자원 요구량을 미리 알아야 함
       5. 프로세스들은 유한한 시간 안에 자원을 반납해야 함

### Deadlock detection (탐지)
 - 위의 방법들을 적용하지 않았을 때, Deadlock이 발생할 수 있으니 이를 찾고 회복하는 알고리즘을 말함
 - Single Instance of Each Resource Type
   - wait-for graph(Resource-allocation graph에서 resource를 뺀 것)을 사용해 탐지 알고리즘 정의
   - 대기 그래프가 사이클을 포함하는 경우에만 시스템에 deadlock이 존재하고, 이를 탐지하기 위해 시스템은 대기 그래프를 유지하고 주기적으로 사이클을 탐지하는 알고리즘을 실행. 이 알고리즘의 복잡도는 O(n^2)
   <img width=60% src="https://user-images.githubusercontent.com/29935109/210230038-255b4b82-0e8b-4199-950e-d6f848507dc3.jpeg">
 - Several Instance of a Resource Type
   - Banker's Algorithm과 유사하게 내용이 달라지는 자료구조를 사용
   - Banker's Algorithm과의 차이점
   <p align="left"><img src="https://user-images.githubusercontent.com/29935109/212645006-af345a18-dc2f-4e18-bce1-935539516a40.png"></p>
   
     - Max가 없고 Need 대신 Request를 사용하여 Banker's algorithm을 실행 -> unsafe state 이면 deadlock으로 판정
     - Need와 Request의 차이점: 
       - Need: 현재 상태에서 요청은 하지 않아도 궁극적으로 작업을 끝내기 위해 필요한 자원의 양
       - Request: 현재 상태에서 특정 자원을 요청한 상황
   - Detection Algorithm(다음주에 추가 예정)
     - 실행하는 시기: 자원 요청을 실패할 때 detection algorithm을 수행하면 deadlock을 유발한 스레드를 식별하는 데 도움이 됨
     - safe sequence가 없는 경우 시간이 지날수록 deadlock 멤버가 늘어나 deadlock recovery가 어려워짐
 - Recovery from Deadlock
   - process termination (종료)
     - Abort all deadlocked Processes
     - Abort one process at a time -> 그 후 deadlock이 풀리는지 계속 check
     - process selection
        1. 프로세스 우선순위
        2. 프로세스가 수행한 시간과 일을 끝마치는데 남은 시간
        3. 프로세스가 사용한 자원 타입과 양
        4. 프로세스가 종료하기까지 남은 자원의 수
        5. 얼마나 많은 수의 프로세스가 끝나야 하는지
        6. 프로세스가 interactive인지, batch 형태인지
   - resource preemption (자원선점)
     - 아래와 같은 순서를 통해 프로세스를 종료시키지 않고 자원만 뺏음
     1. Selecting a victim: 어떤 프로세스의 어떤 자원을 뺏을지 결정
     2. Rollback: 위에서 정한 프로세스로부터 자원을 가져오고 자원을 뺏긴 프로세스를 safe state로 rollback
     3. Starvation: 우선순위에 따라 계속 희생하는 프로세스가 발생할 수 있으므로 이를 방지하기 위해 cost factors에 rollback 횟수를 포함시켜서 계산
       

### Deadlock ignore (무시)
 - 대부분의 OS가 사용 (비용소모 X)
