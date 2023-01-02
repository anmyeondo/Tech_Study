# Creational Design Pattern
기존 코드의 유연성과 재사용을 증가시키는 객체를 생성하는 다양한 방법을 제공한다.
<br/>

## Factory Method
부모 클래스에서 객체들을 생성할 수 있는 인터페이스를 제공한다. 하지만 자식 클래스들이 생성될 객체들의 유형을 변경할 수 있다. 

### 사례
만약 물류 관리 어플리케이션을 제작할 때 `Truck`이라는 클래스에 모든 운송 관련 로직이 있다고 하자,
이 때 해상 관련 기능을 추가하기 위해 `Ship`이라는 클래스를 추가하려면 전체 코드 베이스를 수정해야한다. <br/>
또한 항공 관련 기능이나 다른 기능을 추가하는 등의 변경 사항에 대해 다시 전체 코드 베이스를 수정해야 하는 문제 가 있다. 

### 해결법
팩토리 메서드 패턴은 `new`를 사용한 객체 생성 직접 호출을 특별한 **팩토리** 메서드에 대한 호출로 대체한다.
여전히 객체는 `new`를 통해 생성 되지만 이는 팩토리에서 호출 된다. 또한 이를 통해 생성된 객체를 제품이라고 한다. 

![image](https://user-images.githubusercontent.com/29935137/210215108-f507ec2f-d0e4-4ae9-a473-d72792725a44.png)
<br/>

이 같은 변경 덕분에 이제 자식 클래스에서 팩토리 메서드를 오버라이딩하고 그 메서드에서 생성되는 제품의 클래스를 변경할 수 있다.

하지만, 이 방법도 제한이 있다. 자식 클래스는 다른 유형의 제품을 해당 제품들이 공통 기초 클래스 또는 인터페이스가 
있는 경우에만 반환할 수 있다. `Transport`인터페이스로 `Logistics` 클래스의 `createTransport`팩토리 메서드 반환 유형을 선언해야 한다.

![image](https://user-images.githubusercontent.com/29935137/210216840-aec718d9-3c94-4ba7-834a-d7d169ea9a88.png)

위 사례의 `Truck`과 `Ship` 클래스 모두 `Transport` 인터페이스를 구현해야 하며, 이 인터페이스는 `deliver`라는 메서드를 선언해야한다. <br/>
그러나 각 클래스는 이 메서드를 다르게 구현하게 된다. `RoadLogistics` 클래스에 포함된 팩토리 메서드는 `Truck` 객체를 반환하고, <br/>
`SeaLogistics` 클래스에 포함된 팩토리 메서드는 `Ship` 객체를 반환하게 된다. 

이 과정에서 물류 관리 앱은 여러 제품의 차이를 알지 못하고, 모든 제품을 추상 `Transport`로 간주하여 동작한다.

### 적용
**Q1.** 코드가 함께 작동해야 하는 객체들의 정확한 유형과 의존관계들을 미리 모르는 경우 <br/>
**A1.** 팩토리 메서드는 제품 생성 코드를 제품을 실제로 사용하는 코드와 분리한다. 따라서 제품 생성자 코드를 나머지 코드와 독립적으로 확장할 하기 쉽다.<br/>
<br/>
**Q2.** 라이브러리 또는 프레임워크의 사용자들에게 내부 컴포넌트를 확장하는 방법을 제공할 때 <br/>
**A2.** 프레임워크 전체에서 컴포넌트들을 생성하는 코드를 단일 패토리 맥서드로 줄인 후, 누구나 이 팩토리 메서드를 오버라이드 할 수 있도록 한다.<br/>
<br/>
**Q3** 기즌 객체들을 매번 재구축하는 대신 이들을 재사용하여 시스템 리소스를 절약하고 싶을 때<br/>
**A3** 데이터베이스 연결, 파일 시스템, 네트워크와 같은 시스템 자원을 많이 사용하는 대규모 객체를 처리할 때 사용하며, 이 코드는 주로 생성자에 위치한다.<br/>
그러나 생성자는 항상 새로운 객체들을 반환해야 하며, 기존 인스턴스는 반환할 수 없다. 따라서 새 객체들을 생성하고 기존 객체를 재사용할 수 있는 일반적인 메서드가 필요하다.<br/>
<br/>



### 장단점

#### 장점
- 크리에이터와 구상 제품들이 단단하게 결합되지 않도록 할 수 있다.
- 단일 책임 원칙. 제품 생성 코드를 프로그램의 한 위치로 이동하여 코드를 더 쉽게 유지관리할 수 있다.
- 개방/폐쇄 원칙. 당신은 기존 클라이언트 코드를 훼손하지 않고 새로운 유형의 제품들을 프로그램에 도입할 수 있다

#### 단점
- 패턴을 구현하기 위해 많은 새로운 자식 클래스들을 도입해야 하므로 코드가 더 복잡해질 수 있다. 가장 좋은 방법은 크리에이터 클래스들의 기존 계층구조에 패턴을 도입하는 것이다.


## Singletone
싱글턴은 클래스에 인스턴스가 하나만 있도록 하면서 이 인스턴스에 대한 전역 접근 지점을 제공하는 생성 디자인 패턴이다.

### 문제
싱글턴 패턴은 한 번에 두 가지 문제를 동시에 해결하여 단일 책임 원칙을 위반한다.

**1. 클래스에 인스턴스가 하나만 있도록 한다.** 
 사람들이 클래스에 있는 인스턴스의 수를 제어하려는 이유는 일부 공유 리소스(DB, FILE)에 대한 접근 제어를 위함이다. <br/>
 예를 들어 객체를 생성했지만 잠시 후 새 객체를 생성하기로 했다고 가정한다면, 새 객체를 생성하는 대신 이미 만든 객체를 받게 된다.<br/>

물론 생성자 호출은 특성상 반드시 새 객체를 반환해야 하므로 위 행동은 일반 생성자로 구현할 수 없다.

**2. 해당 인스턴스에 대한 전역 접근 지점을 제공** 
 필수 객체들을 저장하기 위해 전역 변수들을 정의했다고 가정해보자, <br/>
 이 변수들을 사용하면 매우 편리할지 몰라도, 모든 코드가 잠재적으로 해당 변수의 내용을 덮어쓸 수 있고 그로 인해 앱에 오류가 발생해 충돌할 수 있다.<br/>
 
 전역 변수와 마찬가지로 싱글턴 패턴을 활용하면 프로그램 모든 곳에서 일부 객체에 접근 가능하다. <br/>
 
 최근에 싱글턴 패턴이 워낙 대중화되어 나열된 문제 중 하나만 해결해도 그것을 `싱글턴` 이라고 부를 수 있다.
 
 ### 해결책
 싱글턴의 모둔 구현은 공통적인 두 단계를 가진다.
 1. 다른 객체들이 싱글턴 클래스와 함께 `new`연산자를 사용하지 못하도록 디폴트 생성자를 비공개로 설정
 2. 생성자 역할을 하는 정적 생성 메서드를 만든다. 내부적으로 이 메서드는 객체를 만들기 위해 비공개 생성자(private contructor)를 호출한 후 객체를 정적 필드(static)에 저정한다. 이 메서드에 대한 그다음 호출들은 모두 캐시된 객체를 반환

### 실제 적용
Spring의 경우 모든 객체가 Singletone으로 관리 되어 있다. 각각의 객체는 Bean으로서 IoC 컨테이너에서 관리되며 사용자의 필요에 따라 주입받는다.

### 장단점

#### 장점
- 클래스가 하나의 인스턴스만 갖는다는 것을 확신할 수 있다.
- 이 인스턴스에 대한 전역 접근 지점을 얻는다.
- 싱글턴 객체는 처음 요청될 때만 초기화 된다.
- 적은 양의 객체를 관리하여 메모리 공간을 아낄 수 있다.

#### 단점
- 단일책임원칙을 위반한다. 이 패턴은 한 번에 두 가지 문제를 동시에 해결한다.
- 다중 스레드 환경에서 여러 스레드가 싱글턴 객체를 여러 번 생성하지 않도록 `특별한 처리`가 필요하다.
- 싱글턴의 클라이언트 코드를 유닛 테스트 하기 어려울 수 있다. 많은 테스트 프레임워크들이 모의 객체들을 생성할 때 상속에 의존하기 때문이다.
 
## Abtract Factory

## Builder

## Prototype