# 가상 메모리 (Virtual Memory)
메인 메모리의 크기는 한정되어 있기 때문에 물리적인 메모리 크기보다 크기가 큰 프로세스는 실행시킬 수 없다.  
이를 해결하기 위해 나온 것이 가상 메모리 기법이다.  
프로세스를 실행할 때 실행에 **필요한 일부** 만 메모리에 적재하고 나머지는 디스크(보조 기억 장치)에 두는 방법이다.  
**즉, 가상 메모리는 메모리가 실제 메모리보다 많아 보이게 하는 기술이다.**  
각 프로그램에 실제 메모리 주소가 아닌 가상의 메모리 주소를 주어 해당 가상 메모리 공간을 통해 논리적으로 연속성을 가지게 되고 Translation을 통해 실제 물리적 메모리에 매핑되기 때문에 물리적 주소에 대해 알 필요가 없어진다.  
이때 가상적으로 주어진 주소를 가상 주소(virtual adress) 혹은 논리 주소(logical address) 라고 하며, 실제 메모리 상에서 유효한 주소를 물리 주소(physical adress) 라고 한다.  
가상 주소 공간은 **메모리 관리 장치(MMU)** 에 의해 물리 주소로 변환된다.

<p align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6e/Virtual_memory.svg/220px-Virtual_memory.svg.png" width=300px height=600px/>
</p>

## MMU(Memory Management Unit)
  - 가상 주소를 물리 주소로 변환해주는 장치(CPU 코어 안에 탑재됨)
  - 메모리 보호 기능 수행

## 요구 페이징 (DemandPaging)
프로세스를 페이지 단위로 나누어 실행에 필요한 페이지만 메모리에 적재하는 방법. (프로세스를 나누는 단위에는 세그먼트도 있지만 현재는 대부분 페이지 단위 사용)  

## 페이지 폴트 (page faults)
필요한 페이지만 메모리에 적재하기 때문에 어떤 페이지에 접근하려고 했을 때 해당페이지가 실제 물리 메모리에 존재하지 않을 수도 있다. 이러한 경우를 페이지 폴트라고 함.  
페이지 폴트가 발생하면 MMU가 인터럽트(트랩)를 발생시킴.

## 페이지 폴트 과정

<p align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdKWtjd%2Fbtq84OSvgkT%2FxWLi37RZbHLCGzorjwBBI0%2Fimg.png" width=900px height=400px/>
</p>

1. CPU에서 특정 데이터에 대한 가상 주소를 요청
2. MMU에서 해당 주소를 기반으로 TLB 조회 -> 해당 데이터가 있으면 바로 반환
3. TLB에 해당 데이터가 없다면 메인 메모리의 page table에 접근하여 해당 가상 주소의 물리 주소 검색 -> 해당 물리 주소가 메인 메모리에 적재되어 있다면 해당 물리주소 데이터를 바로 반환
4. 메인 메모리에 적재되어 있지 않은 경우, MMU가 page fault 인터럽트 발생시켜 운영체제에 전달
5. 운영체제는 page fault를 전달 받고 디스크에 접근하여 해당 페이지를 메인 메모리에 적재 후 메인 메모리의 page table 업데이트

  - 참고로 TLB(Translation Lookaside Buffer)는 가상 메모리 주소를 물리적인 주소롤 변환하는 속도를 높이기 위해 사용되는 캐시임

## 페이지 교체 알고리즘
FIFO, LRU, LFU는 Cache 교체 알고리즘과 내용이 중복되어 추가 사항만 적음  
이외에도 OPT, MFU 등이 있음

### FIFO
  - Beoady's Anomaly 현상이 발생할 수 있음

#### Beoady's Anomaly
페이지의 프레임의 개수를 늘렸지만 페이지 폴트가 오히려 증가하는 현상
  - 원인 : Locality(지역성)을 고려하지 않은 FIFO의 한계

### OPT(Optimal) 알고리즘
가장 오랫동안 사용하지 않을 페이지를 교체하는 알고리즘
  - 모든 교체 알고리즘 중 페이지 폴트가 가장 적게 일어남
  - 프로세스가 앞으로 사용할 페이지를 미리 알아야 하는 점 때문에 실제로는 구현이 거의 불가능함
  - 실제로 사용하기 보다는 연구 목적을 위해 사용됨

<p align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FS8lUX%2Fbtq9JNyN39f%2FoYoWX91sjF34LkuFfuJxrk%2Fimg.png" width=900px height=400px/>
</p>

### MFU(Most Frequently Used) 알고리즘
LRU와 반대로 참조 횟수가 가장 많은 페이지를 교체하는 알고리즘 -> 즉, 가장 많이 사용된 페이지가 앞으로는 사용될 가능성이 적다는 가정

## Reference
https://devraphy.tistory.com/246
https://doh-an.tistory.com/28
https://ko.wikipedia.org/wiki/%EA%B0%80%EC%83%81_%EB%A9%94%EB%AA%A8%EB%A6%AC
