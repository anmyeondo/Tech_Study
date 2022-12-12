## INDEX
테이블에서 데이터를 조회할 때 속도를 향상 시키기 위해 새로운 공간을 할당한 뒤 특정 자료구조를 통해 정렬하는 것이다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/205790720-52ac65b5-90a9-4140-bbe0-08b8a7988018.png" width=700px height=500px/>
</p>

그림에서는 id로 나와있지만, 실제로는 각각의 테이블이나 레코드의 주소를 가리키고 있다.
<br/>

### 특징
- 보통 `기본키(Primary Key)`의 경우 기본적으로 인덱싱이 되어있다.
- 대표적인 자료구조로 `B-tree`, `B+tree` 가 있고 `Hash`도 활용한다.
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

### Hash
Hash 인덱스는 Hash를 사용하여 인덱스를 생성하는 것이다. 그렇기 때문에 `=`에서 좋은 성능을 보여준다. 하지만 `>`, `<` 등 비교 연산은 사용할 수 없다.
이는 Hash 값을 바탕으로 비교하는 방식이기 때문에 정렬되어 있지 않아 크기를 비교할 수 없기 때문이다. 그래서 DBMS에서는 조인이나 파티셔닝된 테이블을 검색할 때 사용하게 된다.

### B-tree, B+tree
Btree는 Balanced Binary Tree를 기반으로 만들어진 자료 구조이다. 각 노드가 여러 개의 키 값을 가질 수 있게 된다. 또한 각 키에 대응하는 데이터가 노드에 저장된다.
그리고 자식 노드의 최대 개수를 M이라 할 때 M차 B-Tree라고 하며 노드가 최대로 가질 수 있는 Key는 M-1 개이다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/206967507-9f567413-6693-401d-84ca-87ffc2c3b010.png" width=760px height=360px/>
</p>

- 노드의 키와 데이터는 정렬
- 모든 leaf node는 같은 level
- root node는 자신이 leaf node가 되지 않는 이상 최소 2개 이상의 자식을 가짐
- root node가 아닌 노드는 적어도 M/2개의 자식 노드를 지님
<br/>

B+tree의 경우 데이터를 leaf node에만 저장하고 leaf node간의 이동을 허용한다. 그래서 각 노드에 데이터를 저장하지 않기 때문에 더 많은 키를 저장할 수 있게 된다.
또한 풀 스캔의 경우 leaf node만 순회하면 되기 때문에 효율적이다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/206369371-3612438e-18c9-4acb-9686-7ff5fd9d6e2d.png" width=760px height=360px/>
</p>

- leaf node를 제외한 나머지 노드에는 키만 존재
- leaf node 끼리 연결되어 순회 가능
<br/>

### GIN(Generalized Inverted iNdex)
앞서 언급한 B-tree, B+tree 모두 컬럼의 변형 없이 원본 데이터를 키로 사용하게 된다. 그래서 조건에 변형된 컬럼이 들어가게 된다면 인덱스를 사용할 수 없게 된다.
대표적인 예시로 `like` 조건을 탐색할 때가 있다.

```sql
-- Student 테이블의 name을 사용한 인덱스 생성
CREATE INDEX idx_name on Student USING Btree(name);

-- 인덱스가 사용됨
SELECT * FROM Student WHERE name like 'Kim%';

-- 인덱스가 사용되지 않음
SELECT * FROM Student WHERE name like '%Jaehun';
```

위의 예시처럼 `like`를 사용할 때 %가 먼저 나오게 되면 문자열로 정렬된 Btree 인덱스 입장에서는 빠르게 찾을 수가 없다.
이를 위해 `Postgresql`에서 `GIN`를 제공하여 해결할 수 있다. 

GIN은 trgm(tri-gram) 방식을 사용하여 문자열을 3개 단위로 쪼개 인덱스를 만든다 이렇게 되면 3개 이상의 문자에 대해 검색을 할 때 인덱스를 탈 수 있게 된다.
또한 이런 GIN을 사용하기 위해서 Postgresql의 pg_trgm extension을 추가 해야 사용할 수 있다.
<br/>
<br/>

## 다중 컬럼 인덱스
만약 한 개 이상의 컬럼에서 인덱스를 사용하고 싶다면 어떻게 해야할까. 즉, Student 테이블에 name도 인덱스로 사용하고 싶고, grade도 인덱스로 사용하고 싶다면 말이다.
물론 편하게 생각하여 name에 대한 인덱스와 grade에 대한 인덱스를 생성하면 되지 않을까 생각할 수 있다. 이는 DBMS에서 쿼리의 실행게획을 생성할 때 더욱 효과적인 인덱스를 선택해 사용하고 그 뒤에 다른 인덱스를 사용하게 된다. 이것보다 효과적으로 인덱스를 생성할 수 있는 것이 다중 컬럼 인덱스이다.

예를 들면
```sql
CREATE INDEX idx01 ON Students USING Btree (name, grade)
```
이런식으로 인덱스를 생성할 수 있다. 해당 인덱스는 name을 기준으로 grade가 정렬되어 있어서 name 다음에 grade가 나와야 의미가 있다. (순서가 중요하다!)
<br/>
<br/>

## Reference
https://ssocoit.tistory.com/217 <br/>
https://zorba91.tistory.com/293 <br/>
https://ssup2.github.io/theory_analysis/B_Tree_B+_Tree/ <br/>
https://steady-coding.tistory.com/546
