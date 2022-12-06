## INDEX
테이블에서 데이터를 조회할 때 속도를 향상 시키기 위해 새로운 공간을 할당한 뒤 특정 자료구조를 통해 정렬하는 것이다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/205790720-52ac65b5-90a9-4140-bbe0-08b8a7988018.png" width=700px height=500px/>
</p>

### 특징
- 보통 `기본키(Primary Key)`의 경우 기본적으로 인덱싱이 되어있다.
- 대표적인 자료구조로 `B-tree`, `B+tree` 가 있고 특정 상황에서 `GIN`도 활용한다.
- `Select` 뿐만 아니라 `Update`, `Delete`의 경우도 빠르게 진행할 수 있다. 왜냐하면 두 연산을 진행하기 위해선 데이터를 찾아야 하기 때문이다.
- 인덱스는 보통 Where 절에서 `=`,`<`,`>`,`like` 등의 조건을 통해 검색할 때 사용된다. 
<br/>

### 장점
- 앞서 언급한 것과 같이 데이터를 조회할 때 조건절에 있는 데이터를 빠르게 조회할 수 있게 된다.
<br/>

### 단점
- 해당 인덱스를 위해 새로운 공간을 할당해야 하므로 추가적인 공간이 필요하게 된다.
- 인덱스는 항상 테이블과 같은 최신 상태를 유지해야한다. 하지만 `Insert`, `Update`, `Delete`의 경우 기존의 데이터베이스에 수정이 생기는 작업이므로 테이블이 변경되면 인덱스 또한 수정 작업이 추가적으로 진행 된다.
<br/>

### B-tree, B+tree
Btree는 Balanced Binary Tree를 기반으로 만들어진 자료 구조이다. 또한 여기에 차수의 개념이 추가되어 각 노드에 여러 개의 데이터를 저장할 수 있게 된다.
그래서 각 노드에 M개의 데이터가 저장될 수 있다면 M차 Btree라고 한다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/205814367-ef634e01-a113-4257-90e4-68f7c3d300dc.png" width=760px height=360px/>
</p>
<br/>

- node의 데이터는 정렬됨
- 모든 leaf node는 같은 level
- root node는 자신이 leaf node가 되지 않는 이상 최소 2개 이상의 자식을 가짐
- root node가 아닌 노드는 적어도 M/2개의 자식 노드를 지님


B+tree의 경우 B-tree의 단점을 보완하고자 등장한 자료구조이다. 
B-Tree는 탐색을 위해 노드를 찾아 이동해야한다 B+Tree는 


## Reference
https://ssocoit.tistory.com/217
