# Sort

> 목록의 요소를 순서대로 정렬하는 알고리즘

## Selection Sort
---
<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b0/Selection_sort_animation.gif/220px-Selection_sort_animation.gif"></p>

- 개념
    - 제자리 정렬(in-place sorting) 알고리즘의 하나
    - 해당 순서에 원소를 넣을 위치는 정해져 있고, 어떤 원소를 넣을지 선택하는 알고리즘
    - 주어진 리스트에서 최솟값을 찾고, 주어진 자리에 넣은 후, 최솟값을 제외한 리스트로 리스트를 교체
- 특징
    - 장점
        - 자료 이동 횟수가 미리 결정됨
        - in-place sorting
        - 알고리즘이 단순함
    - 단점
        - 불안정한 정렬 방법: 값이 같은 레코드가 있는 경우 상대적인 위치가 변경될 수 있음
- 시간 복잡도
    - 최적: $O(n^2)$
    - 평균: $O(n^2)$
    - 최악: $O(n^2)$
- 공간 복잡도: $O(1)$

## Insertion Sort
---

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/4/42/Insertion_sort.gif"></p>

- 개념
    - 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교
    - 이후 자신의 위치를 찾아 삽입하여 정렬을 완성
- 특징
    - 장점
        - 안정한 정렬 방법
        - 레코드의 수가 적을 경우 복잡한 정렬 방법에 비해 유리
        - 대부분의 레코드가 정렬되어 있을 경우 효율적
        - in-place sorting
    - 단점
        - 비교적 많은 레코드들의 이동을 포함
        - 레코드 수가 많고 레코드 크기가 클 경우 적합하지 않음
- 시간 복잡도
    - 최적: $O(n)$ - 이미 정렬되어 있을 때
    - 평균: $O(n^2)$
    - 최악: $O(n^2)$ - 반대로 정렬되어 있을 때
- 공간 복잡도: $O(1)$

## Bubble Sort
---
<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Bubble_sort_animation.gif/220px-Bubble_sort_animation.gif"></p>

- 개념
    - 서로 인접한 두 원소의 대소를 비교하고, 조건에 맞지 않다면 자리를 교환하며 정렬하는 알고리즘
- 특징
    - 장점
        - 구현이 간단함
        - 안정한 정렬 방법
        - in-place sorting
    - 단점
        - 비효율적인 $O(n^2)$의 시간복잡도
        - 교환 연산이 많이 일어남(교환이 이동보다 복잡)
        - 요소가 최종 정렬 위치해 있어도 교환이 일어날 수 있음
- 시간 복잡도
    - 최적: $O(n^2)$
    - 평균: $O(n^2)$
    - 최악: $O(n^2)$
- 공간 복잡도: $O(1)$

## Shell Sort
---
<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/d/d8/Sorting_shellsort_anim.gif"></p>

- 개념
    - 리스트를 일정한 기준에 따라 분류
    - 연속적이지 않은 여러 개의 부분 리스트 생성
    - 각 부분 리스트를 삽입 정렬을 이용하여 정렬
    - 부분 리스트가 정렬되면 부분 리스트의 개수를 줄여 알고리즘을 반복
- 특징
    - 장점
        - 연속적이지 않은 부분 리스트에서 교환이 일어나므로 더 큰 거리를 이동이 가능
        - 어느 정도 정렬이 된 상태에서 삽입정렬을 수행하기 때문에 삽입정렬보다 빠름
        - 알고리즘이 간단함
        - in-place sorting
    - 단점
        - 불안정한 정렬 방법
        - gap에 따라 시간복잡도의 편차가 큼
- 시간 복잡도
    - 최적: $O(nlogn/nlog^2n)$
    - 평균: Depends on gap
    - 최악: $O(nlog^2n/n^2)$
- 공간 복잡도: $O(n)$

## Heap Sort
---
<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/1/1b/Sorting_heapsort_anim.gif"></p>

- 개념
    - 최대 힙 트리(내림차순)나 최소 힙 트리(오름차순)를 구성해 정렬하는 방법
    - 힙의 root를 제거하고 힙의 마지막 값을 root로 옮긴 뒤 heapify 연산 실행
    - 제거된 root 값을 나열
    - heapify
        - 삽입 노드와 자식 노드를 비교하여 자식 노드 중 더 큰 값과 교환
        - 삽입 노드가 자식 노드보다 크다면 멈춤
- 특징
    - 장점
        - $nlogn$의 빠른 시간 복잡도
        - 가장 크거나 가장 작은 몇개의 값만 필요할 때 탁월한 성능을 보임
        - in-place sorting
    - 단점
        - 불안정한 정렬 방법
- 시간 복잡도
    - Build-Heap: $O(n)$ + 정렬: $O(nlogn)$
    - 최적: $O(nlogn)$
    - 평균: $O(nlogn)$
    - 최악: $O(nlogn)$
- 공간 복잡도: $O(1)$

## Merge Sort
---
<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cc/Merge-sort-example-300px.gif/220px-Merge-sort-example-300px.gif"></p>

- 개념
    - Divide: 리스트를 두 개의 균등한 크기로 분할
    - Conquer: 부분 배열을 정렬
    - Combine: 정렬된 부분 배열들을 하나의 배열에 병합
    - 정렬이 실제적으로 이루어지는 단계는 Combine이다.
- 특징
    - 장점
        - 안정한 정렬 방법
        - 입력 데이터가 무엇이든 간에 정렬되는 시간이 동일: $O(nlogn)$
    - 단점
        - 레코드가 Array라면 추가적인 저장 공간이 필요
        - 레코드들의 크기가 큰 경우 이동 횟수가 많으므로 매우 큰 시간적 낭비를 초래
- 시간 복잡도
    - 비교 횟수: $nlogn$
    - 이동 횟수: $2nlogn$ - 임시 배열에 복사했다가 다시 가져와야 하므로 비교의 2배 
    - 최적: $O(nlogn)$
    - 평균: $O(nlogn)$
    - 최악: $O(nlogn)$
- 공간 복잡도: $O(n)$

## Quick Sort
---
<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6a/Sorting_quicksort_anim.gif/220px-Sorting_quicksort_anim.gif"></p>

- 개념
    - 다른 원소와의 비교만으로 정렬을 수행하는 비교 정렬
    - 배열 가운데서 pivot을 선택
    - pivot을 기준으로 작은 것은 pivot 앞으로, 큰 것은 뒤로 보내어 배열을 둘로 나눔
    - 분할된 두 배열에 대하여 재귀적으로 과정을 반복
- 특징
    - 장점
        - 빠른 시간 복잡도(같은 $nlogn$ 중에서 가장 빠름)
        - 추가 메모리 공간을 필요로 하지 않음
    - 단점
        - 불안정한 정렬 방법
        - 정렬된 리스트에서는 최악의 시간 복잡도를 보여줌
- 시간 복잡도
    - 최적: $O(nlogn)$
    - 평균: $O(nlogn)$
    - 최악: $O(n^2)$ - 이미 정렬되어 있는 경우
- 공간 복잡도: $O(logn)$

## Tim Sort
---

- 개념
    - Insertion sort와 Merge sort를 결합하여 만든 정렬 알고리즘
    - 많은 프로그래밍 언어에서 표준 정렬 알고리즘으로 채택되어 사용
- 특징
    - 장점
        - 최적의 시간 복잡도는 $O(n)$
        - 참조 지역성이 뛰어남(Insertion sort)
        - 추가적인 메모리를 최소화 함
        - 안정한 정렬 방법
    - 단점
        - 오버헤드가 큰 편이기 때문에 간단한 정렬에서는 비효율적
        - 구현 과정이 복잡함
- 시간 복잡도
    - 최적: $O(n)$
    - 평균: $O(nlogn)$
    - 최악: $O(nlogn)$
- 공간 복잡도: $O(n)$

## 정리
---
|이름|최적|평균|최악|공간 복잡도|안정 정렬|in-place sort|
|---|:---:|:---:|:---:|:---:|:---:|:---:|
|Selection Sort|$O(n^2)$|$O(n^2)$|$O(n^2)$|$O(1)$|X|O|
|Insertion Sort|$O(n)$|$O(n^2)$|$O(n^2)$|$O(1)$|O|O|
|Bubble Sort|$O(n)$|$O(n^2)$|$O(n^2)$|$O(1)$|O|O|
|Shell Sort|$O(nlogn/nlog^2n)$|Depends on gap|$O(nlog^2n/n^2)$|$O(1)$|X|O|
|Heap Sort|$O(nlogn)$|$O(nlogn)$|$O(nlogn)$|$O(1)$|X|O|
|Merge Sort|$O(nlogn)$|$O(nlogn)$|$O(nlogn)$|$O(n)$|O|X|
|Quick Sort|$O(nlogn)$|$O(nlogn)$|$O(n^2)$|$O(logn)$|X|O|
|Tim Sort|$O(n)$|$O(nlogn)$|$O(nlogn)$|$O(n)$|O|O|

## Reference
---
https://gmlwjd9405.github.io  
https://gyoogle.dev/blog/algorithm  
https://ko.wikipedia.org/wiki  
https://d2.naver.com/helloworld/0315536  
https://st-lab.tistory.com/276