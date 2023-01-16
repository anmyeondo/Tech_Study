## Sharding
샤딩은 각 DB 서버에서 데이터를 분할하여 저장하는 방식이다. 해당 데이터에 접근할 때는 샤딩키를 사용하여 동적으로 DB 서버를 매핑하는 과정이 필요하다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/212655697-4d2ec7d3-2c94-4e1c-b285-9a507f6c02fb.png" height="600px" width="800px"/>
</p>

파티셔닝과 샤딩의 차이점은 파티셔닝은 동일한 DB서버 내에서 테이블을 분할하는 것이라면, 샤딩은 DB 서버 자체를 분리하는 것이다. 즉, **DB 서버의 부하**를 분산할 수 있다.

### 요구사항
샤딩을 하기 위해서는 다음과 같은 2가지가 필요하다.

- 라우팅을 위해 구분할 수 있는 유일한 키값이 존재
- 설정으로 쉽게 증설이 가능

### 장점
- Sacle-Out이 가능
- 스캔 범위를 줄여서 쿼리 반응 속도를 빠르게 함
- 장애가 샤드 단위로 발생

### 단점
- 프로그래밍 복잡도 증가
- 데이터가 한 샤드로 몰릴 경우(Hotspot), 샤딩이 무의미 
- 잘못 사용할 경우 위험이 큼
- 한번 샤딩 사용시 이전으로 복구가 힘듦


### 모듈러 샤딩(Modular Sharding or Hash Sharding)
PK를 모듈러 연산한 결과로 DB를 라우팅하는 방식이다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/212656500-ade381d7-8e93-4dc2-a7cc-40183d8ea31a.png"/>
</p>

#### 특징
- 데이터가 균일하게 분산
- db를 추가할 때 이미 있는 데이터의 재정렬이 필요

데이터가 늘어남에 따라 샤딩을 추가적으로 해야하는 상황에 큰 부하가 발생하므로 데이터의 크기가 자주 변하지 않는 데이터에 적용한다.

### 레인지 샤딩(Range Sharding)
PK의 범위를 기준으로 DB를 특정하는 방법

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/212656962-57d6c0e9-7514-4658-a299-cd13bba64de9.png"/>
</p>

#### 특징
- 샤딩을 추가할때 재정렬 비용이 적다
- 일부 DB에 데이터가 집중될 수 있다.

샤딩 추가에 큰 비용이 들지 않아서 데이터의 크기 변동성이 클 때 좋은 선택이다.
다만, 데이터가 집중되어 특정 DB에 요청이 몰릴 수 있기 떄문에 적절한 기준이 필요하다.

### 디렉토리 샤딩(Directory Based Sharding)
별도의 조회 테이블을 사용하여 샤딩을 한다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/212657755-f350c182-df4a-4f17-9f32-4f54b4859b91.png"/>
</p>

#### 특징
- 샤딩에 사용되는 시스템, 알고리즘 사용 가능
- 동적 추가가 쉬움
- Read, Write 전에 조회 테이블을 참조하므로 오버헤드가 존재

샤딩은 일반적인 DBMS에서 지원하는 기능이 아니기 때문에 ORM을 사용하거나 DB에 접근 하는 서버에서 구현해주는 작업이 필요하다.
Java의 경우 AbstractRoutingDataSource를 구현하여 해결 가능

## Reference
https://jaehoney.tistory.com/245
