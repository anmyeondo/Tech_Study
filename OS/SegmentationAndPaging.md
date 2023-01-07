# Segmentation & Paging

---

## 메모리 관리 전략 배경
운영체제와 프로세스는 메모리 영역을 할당받아 사용하게 된다.
- 운영체제는 운영체제 메모리 영역과 프로세스 메모리 영역 접근에 제한이 없다.
- 반면 각 프로세스는 독자적인 메모리 영역을 할당받아 사용하게 되고 운영체제 메모리 영역이나 다른 프로세스의 메모리 영역에 접근할 수 없는 제한이 걸려있다.

이러한 연유로 여러 프로세스가 동시에 실행되는 다중 프로그래밍 환경에서 한정된 메모리 공간을 효율적으로 사용하기 위해서는 메모리의 관리가 필수적이다.

### Swapping
메모리 관리 기법의 한 종류로써 표준 Swapping 방식은 Round Robin과 같은 scheduling을 사용하는 다중 프로그래밍 환경에서 다음과 같은 방식으로 이루어진다.
- CPU 할당이 끝난 프로세스 메모리를 보조 기억장치(eg. 하드디스크)로 보내고 (**Swap Out**)
- 다른 프로세스를 메모리로 불러들일 수 있다. (**Swap In**)

이러한 Swapping 과정은 큰 디스크 전송시간을 필요로 하기에 현재에는 메모리 공간이 부족할때만 Swapping 기법을 사용하고 있다.

### Fragmentation (단편화)
프로세스가 메모리에 적재되고 제거되는 작업이 반복되면서 프로세스들이 차지하는 메모리 영역 사이에 활용하지 못할 정도로 작은 자유공간들이 생겨나게 되는데 이를 fragmentation이라고 부른다.

#### 왜 Fragmentation이 발생하는가?
![BaseAndLimit](https://user-images.githubusercontent.com/31722737/211134322-b95659d8-a5d6-4a80-a7e7-dfef4f25146b.png)
- 여기에서는 메모리 할당 방식으로 Base & Limit 을 채택한다고 가정한다.
- Base & Limit 방식은 연속적인 메모리 할당 기법으로써 프로세스들을 물리적 메모리 공간 상에 연속적으로 할당하게 된다.
- 이후 중간에 위치하던 프로세스가 CPU 할당이 끝나 점유하던 메모리 영역을 해제하게 된다.
- 그런데 메모리를 할당 받아야할 다음 프로세스들의 요구 메모리 크기가 모두 중간에 해제된 영역의 크기보다 크다면 해당 영역은 비어있지만 사용할 수 없는 영역이 되어버린다.

#### Fragmentation의 종류
- External Fragmentation(외부 단편화) : 메모리 가용 영역중 사용하지 못하게 된 일부분, 물리적인 메모리(RAM) 사이사이 남는 공간들을 모두 합하면 충분한 공간이 되는 부분들이 분산되어 있을 때 발생한다.
- Internal Fragmentation(내부 단편화) : 프로세스에게 할당된 메모리 공간에 포함되지만 실질적으로 사용되지 않아 남게되는 영역. (Paging 기법을 사용할 때 발생)

#### Compaction (압축)
External Fragmentation을 해소하기 위해 프로세스가 사용하는 영역들을 한 곳으로 몰아서 자유공간을 확보하는 방법이지만 Cost가 커서 잘 사용하지 않는다.

## Paging

### 개요
- 하나의 프로세스가 사용하는 메모리 영역이 연속적이어야 한다는 제약을 없애는 메모리 관리 기법.(Non-Contiguous Memory Allocation)
- External Fragmentation과 Compaction 작업을 해소하기 위해 생겨난 방법론이다.
- Physical Memory는 Frame이라는 고정 크기 단위로 분리되어 있고, Logical Memory는 Page라 불리는 고정 크기 Block으로 분리된다.
- 일반적으로 Frame과 Page의 크기는 동일하다.(마지막 Page의 경우 크기가 다를 수 있음)

### Logical Address VS Physical Address
- CPU가 생성하는 Address를 Logical Address(Virtual Address)라 부른다.
- 메모리가 실제로 취급하는 Address를 일반적으로 Physical Address라 부른다.
- 따라서 프로세스가 메모리 상에 적재되기 위해서는 Logical Address를 Physical Address로 맵핑시켜주는 작업이 필요하다.

### Paging 운영방식
![Paging](https://user-images.githubusercontent.com/31722737/211138822-84c2982e-0e73-4bf2-b8e1-9f23d7f5480b.png)
- 해당 이미지에서 p는 Page Number, f는 Frame Number, d는 offset을 의미한다.
- Logical Address는 p + d, Physical Address는 f + d 구조로 이루어진다.
- Logical Address를 Physical Address로 맵핑하기 위해 메모리 상에서 Page Table을 사용하는데 이는 Continuous Allocation 방식에 비해 속도가 저하되는 요인이 된다.
- 그래서 실제로는 TLB(Table Lookaside Buffer)라는 별도의 Cache를 사용한다.
- Page Table에는 Page Number에 맵핑되는 모든 Frame Number가 저장되므로, 일차원 배열을 사용한다. 즉 Page Number가 index 역할을 한다.
- 하지만 TLB에서는 Page Number와 Frame Number를 일부분만 저장하므로 두 정보를 모두 저장하고 있어야 한다.

### Paging 장단점
#### 장점
- Paging을 사용함으로써 Logical Memory는 Physical Memory에 저장될 때 더 이상 Contiguous하게 이뤄질 필요가 없게된다.
- 즉 Page가 Physical Memory 내에 남는 Frame에 적절하게 배치됨으로써 External Fragmentation을 해결할 수 있게 된다.

#### 단점
- 거의 필연적으로 마지막 Page를 Mapping하는 Frame에는 Internal Fragmentation이 발생하게 된다.
- 이를 해결하기 위해 Frame의 크기를 작게할 수 있는데 이때 반대 급부로 Page Table 크기가 커지게 되므로 Trade-Off를 잘 조절해야 한다.


## Segmentation
### 개요
- Logical Memory 부분을 Paging 기법과 다르게 서로 크기가 다른 논리적 단위로 분할한 것을 의미한다.
- 즉 Segmentation 기법을 사용하면 Physical Memory에 적재되는 Segment들의 size가 모두 다를 수 있다는 것이다.
- 분할된 Segment들을 Physical Address에 Mapping하는 방식은 Paging 기법과 동일하다.

### 장점
- Internal Fragmentation 문제를 해결하고 논리적 단위로 분할하여 저장하기 때문에 중복되는 부분을 여러 프로세스에서 공유할 수 있다는 장점이 있다.
- 또한 논리적으로 중요한 부분과 중요하지 않은 부분을 분리하여 관리하기 때문에 데이터 보호와 공유 기능을 수행하기 쉬워진다고 볼 수 있다.

### 단점
- 하지만 기본적으로 크기가 다른 segment들에 대해 메모리를 동적 할당 해줘야 하기 때문에 External Fragmentation 문제가 발생할 수 있다.

## 절충안
- Segmentation과 Paging 기법을 모두 사용하는 방식을 채택할 수도 있다.

![segmentationAndPaging](https://user-images.githubusercontent.com/31722737/211140627-788cf815-6d43-4168-88bb-5f37e574e47e.jpg)
- 먼저 프로세스를 Segment 단위로 분할한 뒤에 단일 Segment별로 다시 Page 단위로 분할한다.
- 이렇게 Physical Memory에 적재하면 Paging 기법의 장점대로 External Fragmentation을 방지할 수 있다.
- 하지만 이 경우에는 테이블을 2개 거쳐가야 하므로 속도적 측면에서 성능 저하가 있을 수 있다.


<br/>
<br/>
<br/>

---

### 레퍼런스
- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC-%EC%A0%84%EB%9E%B5
- https://peim.tistory.com/22
- https://copycode.tistory.com/108