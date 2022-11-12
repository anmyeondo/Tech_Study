# 쓰레드와 프로세스의 차이

- 프로세스는 각자 프로세스간의 통신에 IPC(Interprocess communication)가 필요하지만 쓰레드는 쓰레드 간의 통신에 IPC가 필요하지 않다.
- 프로세스는 code, data, heap, stack 영역을 각자 보유하지만 쓰레드는 code, data, heap 영역은 공유하고 stack 영역만 각자 보유한다.
- 프로세스는 생성과 context switching에 많은 비용이 들어가지만 쓰레드는 적은 비용이 들어간다.
  - Process Context Switching
    - 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태(context)를 PCB(process controll block)에 저장하고 새로운 프로세스의 PCB 정보를 바탕으로 실행하는것을 의미함.
    - context switching 과정에서 캐쉬 메모리 초기화 등 무거운 작업이 진행되고 많은 시간이 소모되는 등의 오버헤드가 발생함. -> 캐쉬 메모리를 초기화하는 이유는 프로세스 사이에서는 공유하는 메모리가 없기 떄문에 context switching이 발생하면 캐쉬에 있는 모든 데이터를 모두 리셋하고 다시 캐쉬 정보를 불러와야함
    - 
