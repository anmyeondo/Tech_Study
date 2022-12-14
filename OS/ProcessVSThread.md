# 쓰레드와 프로세스의 차이

- 프로세스는 각자 프로세스간의 통신에 IPC(Interprocess communication)가 필요하지만 쓰레드는 쓰레드 간의 통신에 IPC가 필요하지 않다.
- 프로세스는 code, data, heap, stack 영역을 각자 보유하지만 쓰레드는 code, data, heap 영역은 공유하고 stack 영역만 각자 보유한다.
- 프로세스는 생성과 context switching에 많은 비용이 들어가지만 쓰레드는 적은 비용이 들어간다.
  - Process Context Switching
    - 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태(context)를 PCB(process controll block)에 저장하고 새로운 프로세스의 PCB 정보를 바탕으로 실행하는것을 의미함.
    - context switching 과정에서 캐쉬 메모리 초기화 등 무거운 작업이 진행되고 많은 시간이 소모되는 등의 오버헤드가 발생함 -> 캐쉬 메모리를 초기화하는 이유는 프로세스 사이에서는 공유하는 메모리가 없기 떄문에 context switching이 발생하면 캐쉬에 있는 모든 데이터를 모두 리셋하고 다시 캐쉬 정보를 불러와야함
    - 즉, 멀티 프로세스에서 context switching이 너무 자주 일어나게 되면 오버헤드가 증가해 CPU 성능 저하의 원인이 될 수 있다.
  - Thread Context Switching
    - 쓰레드도 프로세스와 마찬가지로 TCB 정보를 바탕으로 context switching을 함.
    - 프로세스와 달리 쓰레드들은 프로세스의 자원들을 공유하기 때문에 속도면에서 훨씬 빠르고 CPU의 부담도 훨씬 덜 가지게 된다.


# 멀티 프로세스와 멀티 쓰레드

- 멀티 프로세싱
  - 하나의 작업을 여러 개의 프로세스가 병렬적으로 처리함
  - 프로세스는 실행될 때마다 각각에 고유한 메모리 영역을 할당해주어야 하기 때문에 프로세스가 많아질 수록 더 많은 메모리 공간을 차지하게 됨
  - context switching 비용이 큼
  - 여러 개의 자식 프로세스 중 하나에 문제가 발생하면 그 자식 프로세스에만 이상이 생기고 다른 프로세스에는 영향을 주지 않음
  - 프로세스간의 통신에 복잡한 IPC를 사용하여 통신해야 함
- 멀티 쓰레드
  - 하나의 프로세스 내에서 여러 쓰레드로 나누어 동시에 실행
  - context switching 비용이 작음
  - 하나의 쓰레드에 문제가 발생하면 같은 메모리를 공유하고 있는 다른 쓰레드에게도 영향을 미침
  - 프로세스 내의 자원을 공유하기 때문에 쓰레드 간 통신이 간단함.
  - 자원을 공유하기 때문에 동기화 문제가 발생할 수 있음
