## DRY (Don't Repeat Yourself)
개발할 때 특정 코드/지식/의도/로직이 중복되지 않게 개발하는 것을 의미

**CASE 1**
```javascript
// Before
function greetings(user) {
	reutrn `Hi ${user.firstName} ${user.lastName}`;
}

function goodbye(user) {
	reutrn `Bye ${user.firstName} ${user.lastName}`;
}

// After
class User{
  fullName() {
    return `${this.firstName} ${user.lastName}`;
  }

function greetings(user) {
	reutrn `Hi ${user.fullName()}`;
}

function goodbye(user) {
	reutrn `Bye ${user.fullName()}`;
}
```

**CASE2**
```javascript
function validInfo(info) {
  if(!info.id) {
    throw new Error('wrong user id');
  }
  
  if(!info.name) {
    throw new Error('wrong user name');
  }
}
```

이와 반대되는 개념으로 WET(Write Every Time)이 있다. 이는 개발자가 실수할 수 있는 환경을 만들기 때문에 지양해야 한다.
<br/>
<br/>

## KISS (Keep It Simple, Stupid)
- 10줄 -> 1줄로 바꿔 가독성을 떨어트릴 바에는 가독성을 높여라
- 함수는 하나의 기능만 사용하도록
- 클래스는 하나의 책임만 가지도록
- UI 컴포넌트는 별도의 BL을 포함하지 않아야 한다
- Service는 단 하나의 기능을 담당하는 간단한 서비스 만들기

**CASE1**
```javascript
// Before
function getFirst(array, isEven) {
	return array.find(x -> (isEven ? x % 2 === 0 : x % 2 !== 0));
}

// After - 1
function getFirst(array, isEven) {
	if (isEven) {
		return array.find(x => x % 2 === 0);
	} else {
		return array.find(x => x % 2 !== 0);
	}
}

// After - 2
function getFisrtOdd(array) {
	return array.find(x => x % 2 !== 0);
}

function getFirstEven(array) {
	return array.find(x => x % 2 === 0);
}
```

**CASE2**
```javascript
// Before
function updateAndInsert(data) {
  // Data Preprocess Logic
  db.update(data);
  // Data Postprocess Logic
  db.insert(data);
}

// After
function update(data) {
  // Data Preprocess Logic
  db.update(data);
}

fuction insert(data) {
  // Data Preprocess Logic
  db.insert(data);
}
```

**CASE3**
```javascript
// Before
class UserOrderService {
  userDb;
  orderDb;
  processUserOrder(userId, orderId) {
    const user = userDb.select(`Query`);
    if (!user){
      thorw Error(`...`);
    }
    const order = orderDb.select(`Query`);
    if (!order){
      thorw Error(`...`);
    }
}

// After
class UserService {
  userDb;
   getUser() {
    return userDb.select(`Query`);
  }
}

class OrderService {
  orderDb;
  createOrder(user, product) {}
  getOrder(orderId) {
    return orderDb.select(`Query`);
  }
}
```
<br/>
<br/>

## YAGNI (You Ain't Gonna Need It)
필요없는 기능, 지금 당장 필요없는 기능, 너무 미래 지향적인 작성을 지양함 물론 확장성 있는 코드를 작성하긴 해야함

**CASE1**
```javascript
function deleteUser(id, softDelete = false) {
	if (softDelete) {
		// softDelte가 필요한지 아닌지도 모르는 상황에서 복잡성이 증가함 만약 softDelete를 하게 되면 select 에도 필터링을 해야함
		return this._softDelete(id);
	}
	return db.removeById(id);
}
```

## Reference
https://www.youtube.com/watch?v=jafa3cqoAVM 드림코딩 유튜브
