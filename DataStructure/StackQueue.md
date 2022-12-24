# Stack & Queue

---

## Stack
- 선형 자료구조의 일종으로 Last In First Out(LIFO) 즉 마지막에 삽입된 데이터가 가장 먼저 호출되도록 한다는 특징을 갖는다.
- 기본적인 사용법은 다음과 같다.

~~~ java
public static void main(String[] args) {
    int[] arr = {10,20,30,40,50,60};
    Stack<Integer> stk = new Stack<>();
    for(int x : arr) stk.push(x);   // Stack에 값 추가하기

    System.out.println(stk.pop());   // Stack 객체에서 가장 상단에 위치하고 있는 값을 삭제하고 그 값을 반환한다.
    System.out.println(stk.peek());  // Stack 객체에서 현재 가장 상단에 위차하고 있는 값을 반환한다. pop()과 달리 실제 값을 Stack에서 삭제하지는 않는다.
    System.out.println(stk.size());  // Stack 객체에 저장된 데이터의 양
    System.out.println(stk.empty());  // Stack 객체에 데이터가 존재하는지 여부를 확인, isEmpty()를 사용해도 결과값은 동일.
    System.out.println(stk.contains(20));   // Stack 객체가 해당 데이터를 저장하고 있는지 확인.
}
~~~

- stack의 활용처
  - 웹 브라우저의 방문기록 (뒤로 가기) : 가장 최근에 방문했던 페이지 순서대로 보여준다.
  - 역순 문자열 만들기 : 가장 나중에 입력된 문자부터 출력한다.
  - 수식의 괄호 검사 : 여는 괄호와 닫는 괄호의 개수를 검사하는데에 사용된다.
  - 함수 콜 스택 : 프로그램이 함수 호출(Function Call)을 추적할 때 사용한다.


- Stack Overflow & Underflow
  - 아무런 데이터 삽입이 없었던 스택에 대해 pop 연산을 수행하려고 할 시 발생하는 에러를 Underflow라고 한다.
  - 정해진 스택 용량이 꽉 찬 상태에서 데이터를 추가로 저장하려고 할 시 발생하는 에러를 Overflow라고 한다.


### Call Stack
- 프로그램이 실행되면서 호출하는 함수들의 정보를 저장한다.
- 함수가 하나 호출되면 해당 호출에 대한 스택이 생성되어 콜 스택에 쌓인다.

~~~ java
import java.util.Random;

public class CallStack {
    private int rollDice(){
        Random randomDice = new Random();
        return randomDice.nextInt(6)+1; // 1~6까지의 숫자들이 무작위로 반환된다.
    }

    private void rollDiceTwiceAndAddSum(){
        int sum = 0;
        sum += rollDice();
        sum += rollDice();
        System.out.println(sum);
    }

    public static void main(String[] args) {
        CallStack T = new CallStack();
        T.rollDiceTwiceAndAddSum();
    }
}
~~~

- 콜 스택 생성 예제코드 실행 순서
![image](https://user-images.githubusercontent.com/31722737/209436513-45ce55b7-8332-4982-bf94-17128a72cf11.png)

<br/>

- 콜 스택에는 어떤 정보들이 저장되는가?
  - 함수 내 지역 변수
  - 함수의 매개변수(Arguments)
  - 호출한 함수(Caller)의 스택에 대한 정보 (Caller 함수의 콜 스택 주소값)
  - 반환 값 주소(Return Address)에 대한 정보 (호출된 다른 함수를 처리한 뒤 어디로 복귀해야 하는지에 대한 정보)

<br/>
<br/>

## Queue
- stack과 마찬가지로 선형 자료구조의 일종이다. First in First Out(FIFO)의 구조를 가지며 먼저 저장된 데이터를 먼저 반환하는 방식으로 데이터를 관리한다.
- 기본적인 사용방식은 아래와 같다.

~~~ java
public static void main(String[] args) {
    int[] arr = {10,20,30,40,50,60};
    Queue<Integer> q = new LinkedList<>();  //자바에서 Queue는 LinkedList 객체로 생성해주어야 한다.

    for(int x : arr) q.offer(x);    // 생성된 Queue 객체에 데이터 삽입(enqueue)

    for(int x : q){
        System.out.print(x + " ");
    }
    System.out.println();

    System.out.println(q.peek());   // Queue 객체에서 가장 앞에 있는 값을 리턴. Queue에서 해당 값을 삭제하지는 않는다.
    System.out.println(q.poll());   // Queue 객체에서 가장 앞에 있는 값을 리턴. Queue에서 해당 값을 삭제한다. (dequeue)
    System.out.println(q.contains(20)); // Queue 객체에서 해당 값이 저장되어 있는지 확인한다.
    System.out.println(q.size());       // Queue 객체가 현재 저장하고 있는 값의 개수를 리턴
    System.out.println(q.isEmpty());    // Queue 객체가 현재 비어있는 상태인지 여부를 확인해서 리턴
}
~~~

<br/>

- queue의 활용처 : 주로 데이터가 입력된 시간 순서대로 처리해야 할 필요가 있는 상황에 이용한다.
  - 우선 순위가 동일한 작업들의 예약
  - 서비스 이용자 대기 리스트
  - 캐시 구현


<br/>
<br/>
<br/>

---

### 레퍼런스
- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#stack-and-queue
- https://devuna.tistory.com/22
- https://ssminji.github.io/2020/02/19/call-stack/
- https://fehead.tistory.com/201