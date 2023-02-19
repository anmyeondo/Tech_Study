# Inter Process Communication

---
## 개요
- 일반적으로 프로세스는 실행될 때 운영체제로부터 독립적인 메모리 영역을 할당받아 실행된다.
- 이로 인해 각 프로세스들은 다른 프로세스의 영향을 받지 않는다는 장점을 갖지만 반대로 별도의 수단이 없으면 통신이 어렵다는 단점이 있다.
- 여러 프로세스가 동시에 실행되며 프로세스간 통신이 필요한 상황이 오게 되는데 이를 해결하기 위해 커널영역에서 IPC(Inter Process Communication)를 제공한다.
- 즉, IPC는 프로세스들이 서로 데이터를 주고받기 위해 통신하는 방식이다.

## IPC 모델 : Shared Memory
### Shared Memory 특징
- 통신하고자 하는 프로세스들이 공유할 수 있는 메모리 영역을 커널에서 할당한다. 이후 공유한 메모리 영역에 read / write 작업을 통해 통신을 수행한다.
- 한 프로세스가 커널에 요청하여 공유 메모리 영역을 할당받으면 이후부터는 커널의 개입 없이 모든 프로세스가 공유 메모리 영역에 접근할 수 있다.
- 즉 한 번 공유 메모리 영역이 할당되면 이후부터는 커널의 개입 없이 프로세스간 통신이 가능해진다.

### Shared Memory 장점
- 초기 이외에는 커널이 프로세스 간 통신과정에 개입하지 않기 때문에 속도가 빠르다.
- 프로그램 레벨 및 유저 레벨에서 통신 기능을 제공하며, 자유로운 통신이 가능하다.

### Shared Memory 단점
- 직접 메세지를 전달하는 것이 아니기 때문에 수신 측 프로세스 입장에서는 언제 공유 메모리 영역에 접근해야 하는지 정확히 알 수 없다.
- 동시에 여러 프로세스가 공유 메모리 영역에 접근하는 경우 문제가 발생할 수 있다.

### 해결방안
- 공유 메모리 영역을 통해 메세지를 송신하고 수신하는 프로세스의 순서를 정해주는 별도의 동기화(Synchronization)기술을 사용한다.
- 공유 메모리 영역에 실제로 접근할 수 있는 프로세스 수를 제한하는 Lock 메커니즘을 사용한다.
- ex) 세마포어, 뮤텍스 등...

<br/>
<br/>

## IPC 모델 : Message Passing
### Message Passing 특징
- 커널 메모리 영역에 프로세스 간 통신을 위한 채널을 따로 만들어서 IPC를 가능하게 하는 방식이다.
- 즉 커널을 경유하여 송/수신 프로세스가 데이터를 주고 받고, 커널은 사이에서 데이터를 버퍼링한다.
- 구현이 간단하고 적은 양의 데이터 전달에 적합하다.

### Message Passing 장점
- 프로세스 간에 따로 메모리 영역을 할당하지 않아도 된다.
- 커널에서 통신을 제어하기 때문에 별도의 동기화 로직 구현이 필요하지 않다.

### Message Passing 단점
- 커널을 경유해서 통신을 하기 때문에 시스템 콜이 필요하며 이로 인한 오버헤드가 발생한다. (Shared Memory 방식보다 속도가 느리다.)

### Message Passing 구현 종류
- 파이프, 메세지 큐, 소켓 등...
- 기본적으로 커널에서 제공하는 Send / Receive 두 가지 연산을 통해 메세지를 주고 받게 된다.
  - 커널을 경유함으로써 발생하는 데이터 복사로 인한 속도 저하
  - send 연산을 사용하기 때문에 메모리를 무한정 줄 수 없고, 만들어진 channel(채널)의 크기 만큼 줄 수 있다.

### Send / Receive 수행 방법 분류
#### 직접 / 간접 통신
- #### Direct Communication
  - 통신하려는 프로세스를 명시하여 메세지를 직접 전달하는 방식이다.
  - 송신 프로세스는 커널에 메세지를 전달할 때 수신 프로세스를 명시한다.
  - 커널은 수신 프로세스에게 메세지를 전달한다.
  - 프로세스 간 링크가 1대1로 이루어지며 양방향으로 대부분 구성된다.

- #### Indirect Communication
  - MailBox 혹은 Port를 통해 메세지를 간접적으로 전달하는 방식이다.
  - 커널에서 제공하는 MailBox / Port에 송신 프로세스가 메세지를 저장한다.
  - 이후 수신 프로세스가 직접 커널의 해당 MailBox / Port에 접근하여 메세지를 수신한다.

<br/>

#### 동기식 / 비동기식 통신
- #### Synchronous Communication (Blocking)
  - Blocking Send : 송신 프로세스는 수신 측이 메세지를 받을 때까지 Block
  - Blocking Receive : 수신 프로세스는 수신할 메세지가 생기기 전까지 Block
- #### Asynchronous Communication (Non-Blocking)
  - Non-Blocking Send : 송신 프로세스는 메세지를 송신 완료 후 수신 여부와 상관없이 자신의 작업(return)을 수행한다.
  - Non-Blocking Receive : 수신 프로세스는 유효한 메세지 혹은 Null을 수신하고 바로 자신의 작업(return)을 수행한다.
- 일반적으로 동기식 통신이 프로그래머 입장에서는 더 사용하기 쉽다.

<br/>

#### 버퍼링 수용 공간에 따른 분류
- #### Zero Capacity
  - buffer의 수용 공간이 0인 경우. 송신 프로세스는 수신 측에서 메세지를 Receive 하기 전까지 Block (랑데뷰 방식)
- #### Bounded Capacity
  - Buffer의 수용 공간이 n인 경우.
  - Buffer가 Whole Free 상태이면 수신 프로세스는 송신 측이 메세지를 Send 하기 전까지 Blcok
  - Buffer가 Whole Full 상태이면 송신 프로세스는 수신 측이 메세지를 Receive하여 공간이 비기 전까지 Block
  - Buffer가 Whole Full/Free 상태가 아니면 송/수신 프로세스는 자유롭게 메세지를 Send/Receive 할 수 있다.
- #### Unbounded Capacity
  - Buffer의 수용 공간이 무한대인 경우.
  - 송신 프로세스는 절대 Block되지 않는다.













<br/>
<br/>
<br/>

---

### 레퍼런스
- https://steady-coding.tistory.com/508
- https://colinch4.github.io/2020-02-02/IPC-%EB%B0%A9%EC%8B%9D/
- https://blog.naver.com/and_lamyland/221177950390
- https://pwnkidh8n.tistory.com/158
- https://junghyungil.tistory.com/146