# 문제 해결 방법

## Divide and Conquer(분할 정복)

> 큰 문제를 작은 문제로 쪼개어 답을 찾아가는 방식으로 하부구조가 반복되지 않는 문제를 해결할 때 사용할 수 있다.

- **Divide**: 문제를 동일한 유형의 여러 하위 문제로 나누는 것, Conquer 가능할 때까지 나눈다.
- **Conquer**: 작은 문제들을 해결한다.
- **Merge**: 작은 문제들의 답을 하나로 합쳐 원래 문제에 대한 결과로 바꾼다.

보통 재귀를 사용하여 아래와 같이 구현한다.
```python
def fibb(n):
    if n <= 1:
        return 1
    return fibb(n-1) + fibb(n-2)
```

## Dynamic Programming(동적 계획법)

> 복잡한 문제를 간단한 여러 개의 하위 문제로(sub-problem)로 나누어 푸는 방법을 말하며 하부구조가 반복되는 문제를 해결할 때 사용할 수 있다.

메모이제이션(Memoization)을 통해 하위 문제의 정답들을 저장해 두었다가 이후 상위 문제에서 활용하는 방식으로 중복 계산을 줄인다.
- 상향식(Bottom-Up): 타뷸레이션(Tabulation)
    - 작은 문제의 정답을 이용해 큰 문제의 정답을 푸는 방식
    - 데이터를 테이블 형태로 저장하기 때문에 `타뷸레이션(Tabulation)`이라고 부른다.
```python
def fibb(n):
    table = [1] * (n+1)
    for i in range(2, n+1):
        table[i] = table[i-1] + table[i-2]
    return table[n]
```
- 하향식(Top-Down): 
    - 같은 하위 문제의 최적해를 저장해서 활용하는 방식으로 중복 계산을 줄일 수 있다.
    - `메모이제이션(Memoization)`이라고 부른다.
```python
def fibb(n):
    if n <= 1:
        return 1
    if table[n]:
        return table[n]
    table[n] = fibb(n-1) + fibb(n-2)
    return table[n]
```
## Greedy(탐욕법)

> 각 단계마다 지금 가장 좋은 방법만을 선택하는 해결방법으로 DP에 비해 수행 시간이 훨씬 빠르다는 장점이 있다.

#### 접근 방법
1. 문제의 답을 만드는 과정을 여러 조각으로 나눈다.
2. 각 조각마다 어떤 우선순위로 선택을 내려야 할지 결정한다. 작은 입력을 손으로 풀어본다.
3. 다음 두 속성이 적용되는지 확인해본다.
    - 탐욕적 선택 속성: 항상 각 단계에서 우리가 선택한 답을 포함하는 최적해가 존재하는가
    - 최적 부분 구조: 각 단계에서 항상 최적의 선택만을 했을 때, 전체 최적해를 구할 수 있는가

## DP vs Divide&Conquer vs Greedy

|Divide and Conquer|Dynamic Programming|Greedy|
|:---:|:---:|:---:|
|non-overlapping한 문제를 작은 문제로 쪼개어 해결하는데 non-overlapping|overlapping substructure를 갖는 문제를 해결한다.|각 단계에서의 최적의 선택을 통해 해결한다.|
|top-down 접근|top-down, bottom-up 접근||
|재귀 함수를 사용한다.|재귀적 관계(점화식)를 이용한다.(점화식)|반복문을 사용한다.|
|call stack을 통해 답을 구한다.|look-up-table, 즉 행렬에 반복적인 구조의 solution을 저장해 놓는 방식으로 답을 구한다.|solution set에 단계별 답을 추가하는 방식으로 답을 구한다.|
|분할 - 정복 - 병합|점화식 도출 - look-up-table에 결과 저장 - 나중에 다시 꺼내씀|단계별 최적의 답을 선택 - 조건에 부합하는지 확인 - 마지막에 전체조건에 부합하는지 확인|
|이분탐색, 퀵소트, 머지소트|최적화 이분탐색, 이항계수 구하디, 플로이드-와샬|크루스칼, 프림, 다익스트라, 벨만-포드|

#### Reference
- [Interview Question for Beginner](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/blob/master/Algorithm/README.md)
- [AI Tech Interview - Algorithm](https://github.com/boostcamp-ai-tech-4/ai-tech-interview/blob/main/answers/8-algorithm.md)