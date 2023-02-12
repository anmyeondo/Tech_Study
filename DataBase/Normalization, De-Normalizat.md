# 정규화 (Normalization)
> 데이터베이스에서 발생하는 이상현상(Anomaly)을 제거할 수 있도록 테이블의 구조를 변경하는 작업.  1~3 정규형과 BCNF 정규형이 있다.<br/>
> <br/>
> **Anomaly** <br/>
> 삽입 이상(Insertion Anomaly) : 튜플 삽입 시 특정 속성에 해당하는 값이 없어 NULL을 입력해야 하는 현상 <br/>
> 삭제 이상(Deletion Anomaly) : 튜플 삭제 시 같이 저장된 다른 정보까지 연쇄적으로 삭제되는 현상 <br/>
> 갱신 이상(Update Anomaly) : 튜플 갱신 시 중복된 데이터의 일부만 갱신되어 일어나는 데이터 불일치 현상 <br/> 
<br/>
<br/>

## 제 1 정규형 (1NF)
1. 각 컬럼에는 하나의 값만 존재한다.
2. 하나의 컬럼 안에서는 같은 타입을 지닌다.
3. 각 컬럼이 Unique한 이름을 가진다.
4. 칼럼의 순서는 상관없다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/218291985-6c4faf2d-e9f4-43b4-b7a0-8bbf8bdf0f7c.png" width="800px" height="300px">
</p>

하나의 컬럼에서 2개의 값이 존재하고 있다. 따라서 다음과 같이 분리할 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/218292095-1539ad56-fa05-4d2d-a595-73056a87ef04.png" width="800px" height="300px">
</p>
<br/>
<br/>

## 제 2 정규형 (2NF)
1. 1NF를 만족한다.
2. 기본키(Primay Key)와 다른 컬럼간의 함수적 종속성이 존재 하지 않는다. 즉, 부분적 종속(Partial Dependency)이 없다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/218292274-c42e9fa4-6518-4d0b-9b48-d01e204b7394.png" width="800px" height="300px">
</p>

위의 테이블에서 기본키는 {학번, 과목}이다. 하지만, 과목과 지도 교수는 서로를 결정하는 함수적 종속성이 존재한다. <br/>
따라서 해당 테이블을 다음과 같이 2개의 테이블로 분리할 수 있다.

<p align="center" position="relative">
  <img src="https://user-images.githubusercontent.com/29935137/218292376-4db5c873-4aea-485a-97af-6c7e458058f5.png" width="500px" height="300px">
  <img src="https://user-images.githubusercontent.com/29935137/218292382-0800b635-c97a-4672-a37d-0ff94c037bf2.png" width="500px" height="300px">
</p>
<br/>
<br/>

## 제 3 정규형 (3NF)
1. 2NF를 만족한다.
2. 기본키가 아닌 다른 컬럼간의 이행 종속성이 존재하지 않는다. 이행 종속성이란 A<B, B<C 일때 A<C를 의미

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/218292744-02dd7d58-1274-4158-b8ce-28cc69d4641f.png" width="500px" height="300px">
</p>

다음 테이블의 기본키는 회원번호이다. 하지만, 회원 등급과 할인률이 서로 함수적 종속성이 존재한다. 따라서 기본키로 등급을 알면 할인률까지 알 수 있는 이행 종속성 관계가 된다.

<p align="center" position="relative">
  <img src="https://user-images.githubusercontent.com/29935137/218292791-e2cf385c-c84d-4ceb-9228-38119b752d3d.png" width="500px" height="300px">
  <img src="https://user-images.githubusercontent.com/29935137/218292804-f0d974b7-3d0e-416a-b2c6-94c7c4037f47.png" width="500px" height="300px">
</p>
<br/>
<br/>

## BCNF (Boyce-Codd Normal Form)
1. 3정규형을 만족해야 한다.
2. 모든 결정자가 후보키 집합에 속해야 한다.

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/218312188-f0c236c0-c748-4b7c-936c-497f5d2ed328.png" width="500px" height="300px">
</p>

위의 테이블의 기본키는 {학번, 과목} 이다. 여기서 과목은 다른 교수가 가르칠 수 있지만, 교수는 가르치는 과목이 정해져 있다. <br/>
따라서 과목을 결정하는 교수가 후보키의 집합에 없기 때문에 다음과 같이 분리할 수 있다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/29935137/218312325-392f85be-baa2-43f3-b95b-4965bf6cec19.png" width="500px" height="300px">
  <img src="https://user-images.githubusercontent.com/29935137/218312351-b965b084-2369-4b4d-8e07-b3cd7bcad36c.png" width="500px" height="300px">
</p>
<br/>
<br/>

## 장점
- 정규화를 진행함으로 이상현상을 방지할 수 있다.
- 데이터베이스의 구조 변경이나 확장시 변경을 최소화할 수 있다.

## 단점
- 필요한 데이터를 조회할 때 Join 연산으로 인한 Overhead가 존재한다.
<br/>
<br/>

정규화를 통해 데이터베이스의 구조를 유연하게 하고 이상현상을 방지할 수 있지만, 테이블이 분리되기 때문에 Join연산이나 다른 Overhead가 존재하게 된다. <br/>
따라서 이런 문제점을 위해 반정규화(De-Normalizat)을 통해 정규화를 진행하지 않는 경우도 있다. 상황에 따라 알맞는 정규화를 진행하면 된다.
\+ 보통은 3NF까지 진행을 하며 BCNF까지는 하지 않는다.
