## 연결 리스트
각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료구조
### 특징
- 메모리가 연속적일 필요가 없으므로 메모리 낭비가 적음
- 포인터만 수정하면 되기 때문에 데이터의 추가와 삭제가 빠름
- 데이터를 조회할 때 포인터들을 따라 앞에서부터 순회하기 때문에 느림
- array list와 linked list의 관계는 trade-off이기 때문에 특징에 따른 선택이 중요함

### 단일 연결 리스트
<p align="center">
<img src="https://w.namu.la/s/c5f4de56c9f7f80fc7e512c3a82c4d9473d5c4dc818d746a7c419f5a6fd60f5f41665a9a9bfab13392b94de8fd600dbb68df89689f81d938d7b6f359215454e5fba47f2809f10c964c7a881d82a8266536a52e71e67590b228e0ed601c1d2df4" width=300px height=100px/>
</p>

- 가장 단순한 형태의 연결 리스트
- 단방향이기에 중간의 Head 주소가 잘못 되면 뒤쪽 자료들을 찾을 수 없음

### 이중 연결 리스트
<p align="center">
<img src="https://w.namu.la/s/e9e9346590be7348d268b24f5a2c2fe8079f23f217a2add5f37c77f87ee60447e55fa25b607fa6fa2bf03d07527cdc46a11613042aba72f62c3db4fbc5cdd9c9df3411b0238d0eef64e20adaaf4cc66fe35e2ee6da63c1a220af3ebd426c788f" width=300px height=100px/>
</p>

- 다음 노드의 주소와 이전 노드의 주소를 모두 가지고 있는 연결 리스트
- 단일 연결 리스트보다는 손상에 강하지만 삽입이나 정렬의 경우 작업량이 더 많다는 단점이 있다.

### 원형 연결 리스트
<p align="center">
<img src="https://w.namu.la/s/412e190e25547fcd6915e0542ccc1fd790cd1eed5e6b6df99a939f35b5b05a238326aa12d5236203ed2f3213759a08a7d2836c3a3429929e695b650b458103e7f2d2899afb8ad76068be1e60be1d5746f70b607659133b7977e2219b23035283" width=300px height=100px/>
</p>

- 단순 연결 리스트에서 마지막 원소가 Null이 아닌 첫 원소를 가리키는 연결 리스트
- 스트림 버퍼나 큐를 사용하는데 사용

### 청크 리스트
- 배열과 연결 리스트의 장점을 합친 것
- 연결 리스트의 원소가 배열
- 청크 리스트의 발전형이 B+tree