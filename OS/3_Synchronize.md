# Synchronize

---

## Synchronization
- 운영체제는 다중 프로그래밍 시스템이기 때문에 여러개의 프로세스들이 존재한다.
- 그렇기에 병렬적으로 수행되고 있는 여러 프로세스들이 공유 자원에 동시에 접근하게 된다면 문제가 발생할 수 있다.
- 이를 위해 각 프로세스들이 서로 순서를 맞춰서 하나의 자원을 하나의 프로세스만이 이용하도록 제어하는 것을 동기화(Synchronization)이라고 한다.

## Race Condition

### Race Condition 개요
- Race Condition은 여러 프로세스들이 동시에 데이터에 접근하는 상황에서, 어떤 순서로 데이터에 접근하느냐에 따라 결과 값이 달라질 수 있는 상황을 말한다.
- Race Condition은 프로세스들의 접근 순서에 따라, 프로세스 간 공유하는 데이터의 불일치성 문제를 야기할 수 있다.
- 따라서, Race Condition 발생을 방지하기 위해서 공유 데이터 자원에 접근하는 프로세스들 간의 실행 순서를 정해주는 동기화(Synchronization)가 필요하다.

### Race Condition이 발생하는 경우

- 커널 : 운영체제에서 한정된 시스템 자원을 효율적으로 관리하여 프로그램의 실행을 원활하게 하는 역할을 수행하는 프로그램

#### 1. 커널모드로 수행 중 인터럽트가 발생하는 경우

<br/>
<p align="center"> <img src="https://user-images.githubusercontent.com/31722737/201508765-c00d965a-3340-4d76-8bcd-fa99ddafd047.jpg"></img> </p>


- 문제 발생 과정
  - count++을 수행하기 위해 count 값을 레지스터 메모리에 load한다.
  - load 이후 Interrupt가 발생하여 Interrupt Handler에 의해 기존 count 값이 -1로 변경된다.(레지스터에 저장된 기존 count 값은 변하지 않는다.)
  - Interrupt Handler 처리 완료 후 다시 count++ 연산을 진행하기 위해 레지스터의 count 값을 1증가 시키고 저장한다.
  - 의도한 count 결과 값은 0이었으나 실제 값은 1이 되므로 문제가 발생한다.

- 해결방법
  - 커널 모드의 수행이 끝나기 전에는 Interrupt를 걸지 못하도록 하는 방법(disable/enable)으로 해결 가능하다.

#### 2. 프로세스가 시스템 콜을 호출해서 커널모드로 수행 중 Context Switch가 발생하는 경우

<br/>
<p align="center"> <img src="https://user-images.githubusercontent.com/31722737/201508770-970896c8-0b40-4b01-9511-bb9bd19ee162.jpg"></img> </p>

- 문제 발생 과정
    - count++을 수행하기 위해 count 값을 레지스터 메모리에 load한다.
    - load 이후 Context Switch가 발생하여 프로세스 B가 커널모드로 진입하며 기존 count 값을 ++한다.(레지스터에 저장된 기존 count 값은 변하지 않는다.)
    - 다시 Context Switch되어 프로세스 A가 count++ 연산을 계속 진행하기 위해 레지스터의 count 값을 1증가 시키고 저장한다.
    - 의도한 count 결과 값은 2 증가된 값이었으나 실제 값은 1만 증가되므로 문제가 발생한다.

- 해결방법
  - 커널 모드를 수행 중일 땐 cpu가 preempty 되지 않도록 하고, 다시 유저 모드로 돌아갈 때 preempty되도록 하여 해결 가능하다.

#### 3. 멀티 프로세서에서 공유 메모리 내 커널 데이터에 접근하는 경우

<br/>
<p align="center"> <img src="https://user-images.githubusercontent.com/31722737/201508772-a75cd872-19eb-44ec-bd88-a77f410c7baf.jpg"></img> </p>

- 문제 발생 과정
  - 어떤 CPU가 마지막으로 count를 저장(store)했는지에 따라 결괏값이 달라진다.

- 해결 방법
  - 커널 내부에 있는 각 공유 데이터에 접근할 때마다 해당 데이터에 대해서만 lock/unlock을 하는 방식으로 해결 가능하다.


## Critical Section
- Critical Section은 코드 상에서 Race Condition이 발생할 수 있는 특정 부분을 말한다. 즉 다시 말하면 공유 데이터에 접근하는 코드 부분을 말한다.
- Critical Section으로 인해 발생하는 문제들을 해결하기 위해서는 다음 조건들을 만족해야 한다.

### 1. Mutual Exclusion(상호 배제)
- 이미 한 프로세스가 Critical Section에서 작업 중이면 다른 모든 프로세스는 Critical Section에 진입하면 안된다.

### 2. Progress(진행)
- Critical Section에서 작업 중인 프로세스가 없는 상황에서, Critical Section에 진입하고자 하는 프로세스가 존재하는 경우 진입할 수 있어야 한다.

### 3. Bounded Waiting(한정 대기)
- 프로세스가 Critical Section에 들어가기 위해 요청한 후부터 실제로 진입하기까지 무한정 기다려서는 안된다.


## Synchronization Algorithms
### - Peterson's Algorithm
#### 개요
~~~ java
do{
   flag[i] = true;  // 진입 의사 표시
   turn = j;  // 다른 프로세스 먼저 입장 시킨다.
   while(flag[j] && turn==j);
   
   {Critical Section}
   
   flag[i] = false;
   {remainder section}
        }while(1);
~~~

- 프로세스 I 가 critical section에 진입하기 위해서 flag[i]를 true로 변경하고 turn을 다른 프로세스에게 넘겨줘 먼저 수행하게 한다.
- 다른 프로세스가 진입 의사가 있고, 현재 critical section내에서 진행 중이면 대기
- 이후 차례가 돌아와 Critical Section에 진입 후 빠져나오면서 flag[i]를 false로 변경

#### 의의와 문제점
- 해당 알고리즘은 Mutual Exclusion, Progress, Bounded waiting 모두 만족한다.
- 하지만 Critical Section 진입을 기다리면서 계속 CPU와 메모리를 낭비하는 Busy Waiting의 문제점이 있다.


## Mutex Locks
- Critical Section 문제를 해결하기 위한 소프트웨어 도구 중 가장 단순한 방법으로 Mutex(Mutual Exclusion) locks이 있다.
- lock이 하나만 존재할 수 있는 locking 메커니즘을 따르므로 이미 다른 프로세스가 Critical Section에서 작업 중이면 다른 프로세스들은 진입하지 못하도록 한다.

~~~ java
acquire(){
  while(!available);  //busy wait
  available = false;
}

release(){
  available=true;
}


do{
  <acquire lock>
  {critical section}
  <release lock>
}while(1);
~~~
- Mutex Locks 방식은 앞서 설명한 Peterson 알고리즘과 같이 Busy Waiting의 단점이 존재한다.
- lock이 풀릴 때까지 계속 확인하면서 프로세스가 대기하는 것을 Spin Lock이라고도 한다.
- Spin Lock은 Critical Section에 진입을 위한 대기시간이 짧을 때, 즉 Context Switching 하는 비용보다 기다리는 게 더 효율적인 특수한 상황을 위해 고안된 것이다.
- 단일 CPU 시스템에서는 lock을 가진 스레드를 풀어주기 위해서 Context Switching이 필수적으로 일어나야 하기 때문에 유용하지 않고 주로 멀티 프로세서 시스템에 종종 사용된다.



## Semaphores
- 세마포어는 Busy Waiting이 필요 없는 동기화 도구이며, 여러 프로세스나 스레드가 Critical Section에 진입할 수 있는 Signaling 메커니즘이다.

### 개요
- 세마포어는 counter를 사용하여 동시에 자원에 접근할 수 있는 프로세스의 수를 제한한다.
- 여기서 사용하는 변수 S는 사용 가능한 자원의 개수를 의미한다.
- 세마포어 변수인 S 는 오직 두 개의 atomic한 연산을 통해서만 접근할 수 있으며, 한 프로세스가 세마포어 변수를 수정할 때 다른 프로세스는 동시에 접근할 수 없다.

~~~ java
wait(S){
  while(S<=0) do no-op;
  S--;
}

signal(S){
  S++;
}
~~~

- wait(S)는 공유 데이터를 획득하는 연산이고, signal(S)는 반납하는 연산이다.

### Block & WakeUp
- Busy Waiting으로 인한 리소스 낭비를 막기 위해 BLock & Waiting 방식을 사용한다.
- Block & Wakeup 방식은 Critical Section으로 진입에 실패한 프로세스를 기다리게 하지 않고 Block 시킨 뒤 Critical Section에 자리가 나면 다시 깨워주는 식으로 수행된다.
- 일반적으로 Busy Waiting이 비효율적이지만, Critical Section이 매우 짧은 경우 Block & Wakeup의 오버헤드가 더 커질수도 있다.

#### 구현

- 세마포어 코드
  - value는 세마포어 변수
  - blockedQueue는 block 처리된 프로세스들이 기다리는 Queue이다.
~~~ java
class Semaphore{
  int value;
  Queue<process> blckedQueue;
}
~~~

- wait, signal 함수

~~~ java
void wait(Semaphore s){
  s.value--;
  if(s.value<0){
    s.blockedQueue.offer(process);
    block();  // 커널에서 block을 호출한 프로세스를 suspend 시키고 해당 프로세스의 PCB를 waitQueue에 넣어준다. 
  }
}


void signal(Semaphore s){
  s.value++;
  if(s.value<=0){ // s.value++을 해줬음에도 0이라는 것은 아직 대기중이 프로세스가 존재한다는 의미.
    process = s.blockedQueue.poll();
    wakeUp(process);  // block된 프로세스를 깨운 뒤에 해당 프로세스의 PCB를 Ready Queue로 이동시킨다.
  }
}
~~~

- wait이나 signal 함수는 같은 세마포어에 대해 두 프로세스가 동시에 실행할 수 없다.
- 만약 wait이나 signal을 수행하는 도중 preempt 된다면 Critical Section Problem에 빠지게 된다.


### Classical Problems of Synchronization
고전적인 동기화 문제로 유면한 세가지 문제
#### 1. Producer-Consumer Problem (Bounded-Buffer Problem)
- 생산자와 소비자 스레드들이 하나의 공유 버퍼를 사용하여 데이터를 주고받는 상황에서 발생하는 문제
- 둘 이상의 생산자가 비어있는 버퍼를 보고 동시에 데이터를 만들어 넣는 경우
- 둘 이상의 소비자가 동시에 동일한 버퍼의 데이터에 접근해 사용하는 경우
- 해결책 : 동시에 버퍼에 접근할 수 없도록 lock을 걸어줘야 한다.

#### 2. Readers-Writers Problem
- 한 프로세스가 데이터를 수정하는 작업을 수행할 때 다른 프로세스가 접근하면 안 되고, 읽는 작업은 여러 프로세스가 동시에 수행 가능하도록 해야하는 문제
- 하나의 lock을 사용하게 되면 데이터의 일관성은 지킬 수 있으나, 여러 프로세스가 동시에 접근해도 되는 읽기 작업의 경우에도 하나의 프로세스만을 강제하기 때문에 비효율성 문제가 발생
- 해결책 : 현재 수정하려고 하는 프로세스가 없다면 모든 Reader 프로세스의 접근을 허용한다. Writer는 리소스에 접근중인 Reader가 하나도 없을 시 접근 가능하며 Writer가 접근 중에는 Reader들은 접근할 수 없다.
  - 계속해서 reader가 들어오거나 writer가 들어오는 경우 한쪽이 무한정 대기해야 하는 경우가 발생한다.(Starvation 문제)
  - 큐에 우선순위를 부여하거나, 타이머를 설정하여 해결할 수 있다.


#### 3. Dining=Philosophers Problem
- 원탁에 앉은 철학자 5명이 각자 양 옆에 놓인 하나의 젓가락들을 사용하여 식사를 해야하는 문제
- 모든 철학자가 식사를 하길 원해서 각자 왼쪽에 놓인 젓가락을 들었다면, 더이상 어떤 작업도 수행할 수 없게되는 DeadLock 상태에 빠지게 된다.
- 해결책 : 5자리 원탁에 4명만 앉게한다, 짝수/홀수 철학자는 각각 왼쪽/오른쪽 젓가락을 먼저 집도록 하는 방법 등이 있다.


## Monitor
- 세마포어의 문제점은 코딩하기 힘들고 실수하기 쉽고, 정확성을 입증하기가 어렵다는 것이다.
- wait과 signal의 순서에 따라 DeadLLock이 발생하거나 Mutual Exclusion이 깨질 수도 있기 때문이다.
- 이를 보완하기 위해 고안된 구조로 Monitor 구조가 있다.
- - Monitor 구조는 동시 수행 중인 프로세스 사이에서 추상 데이터의 안전한 공유를 보장하기 위한 High-level 동기화 구조이다.

<br/>
<p align="center"> <img src="https://user-images.githubusercontent.com/31722737/201512892-76890c8a-3880-4842-a281-b00a51651839.jpg"></img> </p>


### 개념
- Monitor는 공유 데이터 구조, 공유 데이터에 대한 연산을 제공하는 프로시저(procedure), 현재 호출된 프로시저간의 동기화를 캡슐화한 모듈(module)이다.
- 공유 데이터에 접근하기 위해서는 qksemtl 모니터의 내부 Procedure를 통해서만 접근할 수 있도록 한다.
- 동일한 시간엔 오직 한 프로세스나 스레드만 Monitor에 들어갈 수 있다.
- 세마포어와 가장 크게 두드러지는 차이점은 lock을 걸 필요가 없다는 것이다.
- 대신 Monitor Queue를 사용하여 프로세스들을 block 처리해뒀다가 추후 wake up 처리하도록 한다.



---

## Reference
- https://rebro.kr/176
- https://kimcoder.tistory.com/306
- https://galid1.tistory.com/479
- https://ws-pace.tistory.com/25