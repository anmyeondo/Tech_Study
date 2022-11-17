# CPU
  - 중앙 처리 장치라고 함
  - 사용자가 입력한 명령어를 해석하고 연산한 뒤, 그 결과를 제어하는 장치

## 구성요소
  - ALU(Arithmetic Logic Unit)
    - 산술 논리 연산 장치
    - 제어장치 명령에 따라 실제 연산을 수행하는 장치
  - Control Unit
    - 프로그램 명령어를 해석하고 그것을 실행하기 위한 제어신호를 순차적으로 발생시키는 장치
  - Register
    - PC(Program Counter)
      - 다음에 인출할 명령어의 주소를 가지고 있는 레지스터
    - AC(Accumulator)
      - 연산한 결과를 일시적으로 가지고 있는 레지스터
    - IR(Instruction Register)
      - 현재 or 가장 최근에 인출된 명령어를 가지고 있는 레지스터
    - MAR(Memory Adress Register)
      - 기억장치를 출입하는 데이터의 주소를 가지고 있는 레지스터
    - MBR(Memory Buffer Register)
      - 기억장치를 출입하는 데이터를 가지고 있는 레지스터
  - 내부 CPU Bus

## 간단한 CPU 싸이클
![시스템버스](https://user-images.githubusercontent.com/31719854/202533376-92b05c14-81db-469d-a716-f35193e298ee.jpg)
출처 : https://hpclab.tistory.com/1?category=887083
