![OSI](https://velog.velcdn.com/images%2Fcgotjh%2Fpost%2F52907c8c-c149-4943-ad21-3996f44f912f%2F995EFF355B74179035.jpg)

## OSI 7계층

국제 표준화 기구(ISO)에서 개발한 통신에 관한 계층화 표준 모델

계층화를 통해 통신이 일어나는 과정을 단계별로 서술할 수 있고, 문제가 발생할 시 해결이 용이하다.

송신할 때는 7계층에서 1계층으로 내려가며 header를 추가하고 이를 Encapsulation이라 한다.

수신할 때는 1계층에서 7계층으로 올라가며 header를 제거하고 목적지로 전달한다. 이를 Decapsulation이라 한다.

TCP/IP 프로토콜을 보완하기 위하여 만들어졌지만 TCP/IP 프로토콜의 표준화가 지속적으로 이루어짐에 따라 실제 사용되는 것은 TCP/IP 모형이다.



### Layer 7: Application Layer
* 응용프로그램 간의 데이터 송수신을 담당한다.
---
    - 단위(PDU) : 데이터(Data)
    - 주요 프로토콜 : TELNET, FTP, SMTP, HTTP 등
    - 주요 장비 : 해당사항 없음
---
### Layer 6: Presentation Layer
* 데이터 표현방식, 상이한 부호체계 간의 변화에 대해 규정한다.
* 인코딩/디코딩, 압축/해제, 암호화/복호화 등의 역할을 수행한다.
---
    - 단위(PDU) : 데이터(Data)
    - 주요 프로토콜 : 해당사항 없음
    - 주요 장비 : 해당사항 없음
---
### Layer 5: Session Layer
* 응용 프로그램간의 논리적인 연결(세션) 생성 및 제어를 담당한다.
### Layer 4: Transport Layer
* 종단간 신뢰성 있는 데이터 전송을 담당한다. (End-To-End Reliable Delivery)
* 종단(Host)의 구체적인 목적지(Process)까지 데이터가 도달할 수 있도록 한다. (Process-To-Process Communication)
* Process를 특정하기 위한 주소로 Port Number를 이용한다.
* 신뢰성 있는 데이터 전송을 위해 분할과 재조합, 연결제어, 흐름제어, 오류제어, 혼잡제어를 수행한다.
---
    - 단위(PDU) : 세그먼트(Segment)
    - 주요 프로토콜 : TCP, UDP
    - 주요 장비 : L4 Switch
---
### Layer 3: Network Layer
* 종단간 전송을 위한 경로 설정을 담당한다. (End-To-End 혹은 Host-To-Host Delivery)
* 호스트로 도달하기 위한 최적의 경로를 라우팅 알고리즘을 통해 선택하고 제어한다.
* 종단간 전송을 위한 주소로 IP주소를 사용한다.
---
    - 단위(PDU) : 패킷(Packet)
    - 주요 프로토콜 - IP, ARP, ICMP, IGMP, RIP, RIP v2, OSPF, IGRP, EIGRP, BGP 등
    - 주요 장비 : 라우터(Router), L3 Switch
---
### Layer 2: DataLink Layer
* 인접한 노드간의 신뢰성 있는 데이터(단위 : 프레임) 전송을 제어(Nod-To-Nod Delivery)
* 네트워크 카드의 MAC(Media Access Control)주소를 통해 목적지를 찾아간다.
* 신뢰성 있는 전송을 위해 흐름제어(Flow Control), 오류제어(Error Control), 회선제어(Line Control)을 수행한다.
* 논리링크제어계층, 매체접근제어계층이라는 두 개의 부계층으로 나뉜다.
---
    - 단위(PDU) : 프레임(Frame)
    - 주요 프로토콜 : HDLC, X.25, Ethernet, TokenRing, DFFI, FrameRelay 등
    - 주요 장비 : 브리지(Bridge), L2 Switch 등
---
### Layer 1: Physical Layer
* 물리적인 장치의 전기적, 전자적 연결에 대한 명세
* 디지털 데이터를 아날로그적인 전기적 신호로 변환하여 물리적인 전송이 가능케 한다.
* 주소 개념이 없으며 연결된 노드간에 신호를 주고 받는다.
---
    - 단위(PDU) : 비트(Bit)
    - 주요 프로토콜 : X.21, RS-232 등
    - 주요 장비 : 허브(HUB), 리피터(Repeater) 네트워크 카드(NIC : Network Interface Card) 등
---


## TCP/IP 4계층
현재 인터넷에서 컴퓨터들이 서로 정보를 주고 받는데 쓰이는 프로토콜의 모음

OSI 7계층은 네트워크 전송 시의 데이터 표준을 정의한 것이고 이것의 실무적 표준이 TCP/IP 4계층이다.
### Application
* 사용자와 가장 가까운 계층으로 사용자가 소프트웨어 application과 소통할 수 있게 해준다.
* 응용프로그램(application)들이 데이터를 교환하기 위해 사용되는 프로토콜
* 사용자 응용프로그램 인터페이스를 담당
---
    - 단위(PDU) : 데이터(Data)
    - 주요 프로토콜 : TELNET, FTP, SMTP, HTTP 등
    - 주요 장비 : 해당사항 없음
---    
### Transport
* 통신 노드 간의 연결 제어 및 자료 송수신을 담당
* 애플리케이션 계층의 세션과 데이터그램 통신서비스 제공
* 세그먼트 (Segment)단위의 데이타 구성
    * 실질적인 데이터 전송을 위해 데이타를 일정 크기로 나눈 것. 발신, 수신, 포트주소, 오류검출코드가 붙게된다.
---
    - 단위(PDU) : 세그먼트(Segment)
    - 주요 프로토콜 : TCP, UDP
    - 주요 장비 : L4 Switch
---
### Internet
* 네트워크상 최종 목적지까지 정확하게 연결되도록 연결성을 제공
* 단말을 구분하기위해 논리적인 주소(Logical Address) IP를 할당
    * 출발지와 목적지의 논리적 주소가 담겨있는 IP datagram이라는 패킷으로 데이타를 변경
    * 데이터 전송을 위한 주소 지정
* 라우팅(Routing) 기능을 처리
    * 경로 설정
* 최종 목적지까지 정확하게 연결되도록 연경성 제공
* 패킷단위의 데이타 구성
    * 세그먼트를 목적지까지 전송하기 위해 시작 주소와 목적지의 논리적 주소를 붙인 단위. 데이타 + IP Header
---
    - 단위(PDU) : 패킷(Packet)
    - 주요 프로토콜 - IP, ARP, ICMP, IGMP, RIP, RIP v2, OSPF, IGRP, EIGRP, BGP 등
    - 주요 장비 : 라우터(Router), L3 Switch
---
### Network Interface
* 물리적으로 데이타가 네트워크를 통해 어떻게 전송되는지를 정의
    * 논리주소(IP주소 등)이 아닌 물리주소(예. MAC주소(Media Access Control Address))을 참조해 장비간 전송
    * MAC 주소란 컴퓨터의 하드웨어 주소
* 기본적으로 에러검출/패킷의 프레임화 담당
* 프레임(Frame)단위의 데이타 구성
    * 최종적으로 데이타 전송을 하기 전 패킷헤더에 MAC 주소와 오류 검출을 위한 부분을 첨부
---
    - 단위(PDU) : 비트(Bit)
    - 주요 프로토콜 : X.21, RS-232 등
    - 주요 장비 : 허브(HUB), 리피터(Repeater) 네트워크 카드(NIC : Network Interface Card) 등
---