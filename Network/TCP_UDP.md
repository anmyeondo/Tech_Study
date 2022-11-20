# TCP(Transmission Control Protocol)
Transfer Layer에서 사용하는 전송 방식으로 통신하기 전 연결을 통해 통신의 대상을 정하게 된다.

## TCP의 특징
- 연결 지향형
- 신뢰 있음
- 흐름 제어, 혼잡 제어
<br/>

### 연결 지향형
통신을 시작하기 전 3-Way-Handshake 라는 방식을 사용해 미리 연결을 설정하는데, 먼저 Sender에서 SYN bit가 담긴 작은 패킷을 전송한다.<br/>
그러면 Receiver에서 SYN ACK를 통해 데이터를 받을 준비가 되었다고 다시 패킷을 전송하게 된다.<br/>
Sender 쪽에서 SYN ACK를 수신하면 그것에 대한 ACK를 보내고 통신이 시작되게 된다. <br/>
이 때, SYN ACK에 대한 ACK를 보낼 때 주로 다음에 필요한 데이터의 요청까지 같이 보내게 된다.
<br/>

<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/202894607-53cf56ab-7640-4823-806b-2035acf31a5d.png" width="400px" height="400px">
</p>

<br/>

통신을 마무리하기 위해서는 4-Way-Handshake 라는 동작을 사용한다. 
이것 또한 통신을 끊으련느 쪽에서 먼저 FIN bit가 담긴 작은 패킷을 보내게 된다. 그렇게 되면 상대측에서 이것에 대한 응답으로 FIN ACK를 다시 보내게 된다. 그 이 후 통신 중단을 요청받은 클라이언트에서 보낼 요청이 모두 마무리 되면, FIN 패킷을 보내게 된다. 그러면 이것에 대한 응답을 받은 클라이언트는 FIN ACK를 보내고 통신을 마무리하게 된다.
<br/><br/>
연결 종료시 4Way로 진행하는 이유는 Sender에서 미리 보낸 패킷이 지연으로 인해 도착하지 않았을 경우 Receiver에서 받아야 하는 모든 패킷을 받은 뒤 연결을 종료해야 하기 때문에 Receiver 측에서 모든 패킷을 받은 뒤 연결을 종료 하는 것이다.
<br/>
<p align="center">
<img src="https://user-images.githubusercontent.com/29935137/202894617-93f64c45-0782-418f-8328-2d22c2eb0dd7.png" width="400px" height="400px">
</p>
<br/>
위에서 언급한 방식을 통해 TCP는 연결 지향형 통신방식을 진행한다.
<br/>

### 흐름제어
흐름제어란 데이터 패킷이 sender에서 Receiver로 순서대로 잘 도착 햇는지 확인 하는 것이다.

TCP의 흐름 제어를 위해서는 Stop-and-Wait 방식과 Sliding Window 방식이 존재한다.
Stop-and-Wait 방식의 경우 패킷을 하나 전송하고 그에 대한 알맞은 ACK가 돌아올 경우 다음 패킷을 전송하는 방식이다.

Sliding Window의 경우 일정 Window Size만큼 패킷을 전송하고 그에 대한 ACK가 돌아올 경우 WIndow를 옆으로 밀어 다음 전송해야 할 패킷을 보내는 방식이다.
<br/>

### 혼잡제어
라우터나 서버에 데이터 패킷이 몰려 지연 되는 현상을 혼잡하다고 한다.

TCP의 경우 AIMD(Additive Increase / Multicative Decrease), Slow Start, Fast Retransmit, Fast recovery 방식이 존재한다.
<br/>

#### AIMD
이는 위에서 언급한 Window SIze를 1로 시작하여 패킷을 주고 받는 것에 성공할 경우 크기를 하나 씩 늘리는 방식이다.
<br/>

#### Slow Start
AIMD 방식은 한 싸이클 마다 크기가 1씩 증가하기 때문에 전송 속도가 너무 느리다는 단점이 존재, 그래서 ACK 하나 당 1씩 크기를 늘리는 방식 등장,
즉, 한 사이클당 2배씩 크기가 커지게 된다. 대신 문제가 발생하게 된다면(패킷 유실) 다시 window size를 1로 줄인다.
<br/>

#### Fast Retransmit
정상적인 사이클이 돌아가려면 ACK는 다음 받아야할 데이터의 순서를 담아서 보내게 된다. 하지만 동일한 데이터를 요구하는 ACK가 반복적으로 온다면, 이는 중간에 패킷이 하나 유실 된것을 의미한다. 이럴 경우 ACK가 다 오는것을 기다리지 않고, 3번 같은 ACK가 올 때 다시 전송을 시작하는 방식을 말한다
<br/>

#### Fast Recovery
패킷 유실이 발생했을 때 윈도우 사이즈를 1로 줄여버리면 다시 많은 양의 데이터를 전송하는 데까지 오랜 시간이 걸리게 된다. 그렇기 때문에 패킷 유실이 발생할 때
윈도우 사이즈를 1로 줄이는 것이 아니라 절반으로 줄여서 빠르게 window size를 키우는 방식이다.
<br/>

## TCP를 사용하는 곳
- HTTP
- FTP
- SMTP
<br/>

# UDP(User Datagram Protocol)
위에서 언급한 TCP의 경우 연결 지향형 방식으로 데이터의 신뢰성을 보장할 수 있었다. 하지만, UDP의 경우 데이터 패킷을 전송하고 해당 패킷이 유실되는 등의 문제가 발생해도 다시 전송하지 않는다.

## UDP의 특징
- 비 연결 지향형
- 신뢰 없음
- 헤더가 단순(TCP에 비해)


## UDP를 사용하는 곳
- 게임
- 실시간 스트리밍
- DNS
- HTTP 3.0

# Reference
https://mangkyu.tistory.com/15
