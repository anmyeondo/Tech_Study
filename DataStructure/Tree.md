## Graph
노드와 간선을 통해 데이터를 나타낸 자료구조이다. 주로 네트워크 관계를 나타낼 때 사용된다.

### 특징
1. 방향성, 무방향성 둘다 존재한다.
2. 사이클이 존재할 수 있다.
3. 노드 사이의 간선의 제한이 없다.
4. 루트 노드, 부모 자식의 개념이 없다.
5. DFS, BFS를 활용하여 순회한다.
6. 간선에 가중치를 부여할 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/209455945-18aef8de-b973-46ad-ac4d-240bb0d4d4b9.png"></img>
</p>

## Tree
Graph의 종류 중 하나로 노드와 간선을 통해 데이터를 나타낸다.

### 특징 
1. 위에서 아래로 진행되는 방향성을 지니고 있다.
2. 사이클이 존재하지 않는다.
3. 노드끼리는 하나의 간선으로 연결되어 있다. 즉, 간선은 개수는 무조건 `(Node의 수) - 1` 이다.
</br>

### Binary Tree
부모 노드가 가질 수 있는 자식 노드의 수가 최대 2개인 트리이다.
</br>

#### Full Binary Tree
부모 노드가 가진 자식 노드의 개수가 0 or 2 개인 트리 구조
모든 leaf 노드의 수는 internal 노드+1 이다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/209455667-3fc79c13-6328-4f70-b2e5-7a6b64c4e7a8.png"></img>
</p>

#### Complete Binary Tree
마지막 레벨을 제외 한 나머지 노드는 모두 가득 차있고, 마지막 레벨의 노드는 왼쪽부터 차있는 트리 구조

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/209455705-07d7b9c9-dee7-4571-b581-730741dbe493.png"></img>
</p>

#### Binary Search Tree
이진 트리 구조를 따르며, 키가 존재한다. 부모 노드의 키보다 작은 키는 왼쪽 부모 노드보다 큰 키는 오른쪽으로 둔다.
해당 자료구조를 활용하면 데이터의 검색에서 O(log2N)의 속도를 가질 수 있다.
하지만 한쪽으로 편향된 경우 O(N)이다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/209455844-f30a7c29-3a24-446b-b7fa-dcf9628632fe.png"></img>
</p>

#### Heap Tree
CBT를 따르며 일반적으로 MaxHeap, MinHeap으로 구분 된다.
MaxHeap의 경우 부모 노드의 키는 자식의 키보다 커야하며, MinHeap의 경우 부모 노드는 자식 노드의 키보다 작아야한다.

우선순위 큐를 구현할 때 주로 사용된다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/209455889-0d53a437-85c3-4d50-a3e2-b6185201aecc.png"></img>
</p>


## Reference 
https://yaboong.github.io/data-structures/2018/02/10/1_binary-tree-1/ </br>
https://yoongrammer.tistory.com/71 </br>
https://passwd.tistory.com/321 </br>
https://leejinseop.tistory.com/43 </br>
