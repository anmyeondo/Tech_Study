## Process
우리가 작성한 프로그램은 처음에 하드디스크에 저장되어 있다. 프로그램을 실행하면 CPU에 의해 하드디스크에서 메모리로 올라가며 실행 가능한 상태가 되고 이것을 프로세스라고 부른다.
<br/>
<br/>

## Process의 구성
프로그램을 실행 시키기 위해 메모리에 적재할 때 크게 Stack, Heap, Data, Code의 4가지 영역으로 나뉜다. 

<br/>
<p align="center"> <img src="https://user-images.githubusercontent.com/29935137/200123346-30c28cee-e2f8-4e9d-8f5e-67ce1af67213.png"></img> </p>


### Stack
 함수 안에서 선언된 지역변수, 매개변수, 리턴값, 돌아올 주소 등이 저장되고 함수 호출 시 쌓였다가, 종료 되면 제거하게 된다.
 흔히 말하는 함수의 Call Stack 등의 의미가 이 영역에 데이터가 저장되는 것을 의미한다.

### Heap
 코드 내에서 동적으로 할당된 변수를 위해 제공되는 메모리 영역이다. C에서 malloc 함수를 통해 메모리를 할당하게 되면, 이 영역에 저장되게 된다.
 단, 직접 할당한 공간은 직접 해제 하도록 관리되어야 한다. 그렇지 않으면 메모리 누수로 인해 프로세스가 종료될 수 있다.

### Data
 Global, Static 변수가 저장되는 영역이다. 여기서 할당된 변수는 그대로 Data 영역에 저장되고, 할당되지 않은 변수는 BSS영역에 저장되게 된다.
 
### Code
 실행할 코드가 담겨있는 영역이다. 코드 명령문들은 CPU에 의해 하나씩 불러지며 실행된다.
<br/>
<br/>

## PCB
프로세스를 실행하기 위해 메모리에 적재되는 정보는 위에 나온 것이 전부가 아니다. 프로세스를 다루기 위해 필요한 정보는 따로 OS 커널에 관리되어 메모리에서 사용 되는데,
이를 PCB(Process Controll Block)이라고 하며, 각각의 PID와 PCB를 매핑하여 관리한다.

<br/>
<p align="center"> <img src="https://user-images.githubusercontent.com/29935137/200122308-ba6793d4-c3f7-48be-85ea-e37ab6f0a3c8.png"></img> </p>


* **Process Number(PID)**<br/>
OS에서 각각의 프로세스를 구분하기 위한 식별번호, 이를 이용해 프로세스를 관리할 수 있다.

* **Process State**<br/>
프로세스의 상태를 나타내는 곳이다. New, Ready, Running, Waiting, Terminated 로 구분할 수 있다. <br/>
New - 프로세스가 생성된 상태 <br/>
Ready - 프로세스가 실행되기 위해 Ready Queue에 들어간 상태 <br/>
Running - 프로세스가 CPU를 할당받아 동작을 실행하고 있는 상태 <br/>
Wating - I/O 같은 특정 이벤트로 인해 대기하고 있는 상태 <br/>
Terminated - 프로세스의 실행을 완료한 상태 <br/>

* **Program Counter(PC)**<br/>
현재 프로세스가 어디까지 실행 됐는지 정보를 나타낸다. 즉, 프로세스가 CPU의 자원을 할당 받은 뒤 다음 실행 해야할 명령의 위치를 알림

* **Memory Limit**<br/>
 해당 프로세스의 주소 공간에 대한 정보
 
 * **Register**<br/>
  Context Switching이 일어날 때 이전 프로세스를 처리하던 CPU가 사용하던 저장공간에 있던 정보를 저장
 
 * **Pointer**<br/>
 부모프로세스에 대한 포인터, 자식 프로세스에 대한 포인터, 프로세스가 위치한 메모리 주소에 대한 포인터, 할당된 자원에 대한 포인터 등의 정보가 담겨있다.
 
 * **Open File List**<br/>
프로세스에서 사용하는 파일을 관리하기 위한 정보를 저장하는 공간이다. 흔히 fopen등을 통해 생성된 file descriptor가 저장된다. 또한 프로세스가 실행될 때 자동으로 stdin, stdout, stderr이 열리게 ㅗ딘다.

## Multi Process
우리가 실제로 사용하는 컴퓨터는 한 번에 하나의 프로세스만 실행하는 것이 아닌 여러 개의 프로세스를 동시에 실행하게 된다. 하지만 OS는 이를 병렬로 처리하지 않는다.<br/>
그럼에도 불구하고 동시에 프로세스가 실행되는 것처럼 보이는 이유는 동시성(concurrency)에 의해서 이다.

동시성이란, 동시에 실행되는 것이 아니지만 동시에 실행되는 것처럼 보이는 것이다. 실제로 컴퓨터는 여러개의 프로세스를 번갈아가며 매우 빠르게 실행하기 때문에 동시에 실행되는 것처럼 보이는 것이다.

위에서 말한 동시성을 위해 OS는 `Context Switching`이란 기법을 사용한다.

### Context Switching
프로세스를 실행하다 다른 프로세스의 실행이 필요할 경우 현재 진행하던 프로세스를 정지하고, 또다른 프로세스를 실행하는 방법이다. 이 때 프로세스의 진행 정보는 PCB의 PC에 저장되게 된다. 또한 Context Switching은 cache 초기화, memory mapping 초기화 등으로 인해 비용이 많이 드는 작업이다.

## Reference
https://bowbowbow.tistory.com/16 <br/>
https://enlqn1010.tistory.com/30 <br/>
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jooda99&logNo=220693179391 <br/>
https://nesoy.github.io/articles/2018-11/Context-Switching <br/>
