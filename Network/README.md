# Network

## Internet Protocol Stack
### OSI 7 계층 모델
* PDU(Process Data Unit)
    - 각 계층의 데이터 전송 단위. 계층마다 다르다.
* 네트워크 통신 과정
    1. Physical
        - 기계어를 전기 신호로 바꾸어 물리 매체에 보내는 것
        - PDU : bit
        - Protocol: Modem, Cableetc
        - 장비: 허브, 리피터
    2. Link
        - 네트워크 기기들 사이에서 데이터 전송을 하는 역할
        - PDU : frame
        - Protocol: ethernet, wifi
        - 장비: 브릿지, 스위치
    3. Network
        - 기기에서 데이터가 갈 경로를 설정
        - PDU : Packet
        - Protocol: IP, ICMP
        - 장비: 라우터
    4. Transport
        - 패킷 전송의 유효성 등 통신 신뢰성 보장
        - PDU: Segment
        - Protocol: TCP, UDP
        - 장비: 게이트웨이
    5. Session
        - 통신 세션을 구성
        - 포트 기반으로 연결
        - Protocol : NetBIOS, SSH
    6. Presentation
        - 데이터의 형식을 정하는 과정
        - Protocol: JPEG, MPEG
    7. Application
        - 데이터를 사용자에게 전달(파일 전송 etc)
        - Protocol: DNS, HTTP
### TCP/IP 4 계층
* 요즘에 더 많이 쓰이는 구조
* 데이터의 정확성은 TCP가, 패킷의 전송은 IP가 담당
* 단계별로 헤더를 붙여서 전송하게 된다.
    - Data -> Segment -> Datagram -> Frame
* 계층
    1. Link
        - Physical + Link
    2. Internet(IP)
        - Network
    3. Transport(UDP, TCP)
        - Transport
    4. Application
        - Session + Presentation + Application

## Transport Layer
* IP에 의해 전달되는 패킷의 오류 검사, 전송 제어 담당
### TCP(Transmission Control Protocol)
* 신뢰성을 보장하는 연결형 서비스(연결 해야 통신 가능)
* byte stream으로 연결
* 혼잡/흐름 제어
* 순서 보장, 속도 느림
* TCP packet : Segment
* HTTP, Email, File Transfer에 사용
* 전이중(full-duplex) : 양방향으로 동시에 일어날 수 있음
* 점대점(point to point) : 각 연결이 정확히 2 개의 종단점을 가지고 있음
> TCP 는 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.

#### TCP 연결 과정
* 3-way handshaking
    * 통신 전 접속 확인 하는 방법
    * <사진>
* 4-way handshaking
    * 통신 후 연결 해제 하는 방법
    * 사진
#### 흐름 제어
* 데이터를 너무 빠르게 보내 문제가 생기는 것을 방지
* Stop & Wait: 전송한 패킷에 대한 확인 응답을 받아야 그 다음 패킷을 전송.
* Sliding Window: Segment 별로 전송. 전달이 확인 되면 그 window를 움직임.
#### 혼잡 제어
* 네트워크 내의 패킷 수가 넘치게 증가하는 것을 방지
    1. slow start: window size를 지수적으로 조금씩 늘려서 보내는 것
    2. fast retransmit: 중복 순번이 들어왔다는 것은 혼잡하다는 뜻. 재전송하고 window를 줄인다.
    3. fast recovery: 혼잡할 때 줄였던 window를 선형적으로 증가시키는 것.
#### 헤더

### UDP(User Datagram Protocol)
* 비연결형 프로토콜(연결 없이 통신 가능)
* message stream으로 연결
* 혼잡 / 흐름 제어 없음
* 순서가 보장되지 않고 TCP보다 빠름
* 데이터 전송 보장 X
* UDP packet: Datagram
* DNS, 실시간 스트리밍에 사용

#### 헤더

## Application Layer
* 전송 받은 내용을 display하는 layer
### GET and POST
* HTTP 프로콜로 서버에게 무언가를 요청할 때 사용.
#### GET
* HTTP Request Message의 Header 부분에 url이 담겨서 사용.
* url ? 뒤에 데이터를 붙여 보냄
* 크기가 제한적, 보안 없음(url에서 다 노출)
* 서버에서 조회 등을 위한 목표로 사용
* 요청이 브라우저에 캐슁 될 수 있다. 

#### POST
* HTTP Request Message의 Body 부분에 url이 담겨서 사용.
* url ? 뒤에 데이터를 붙여 보냄
* 데이터 크기가 더 크다.
* 서버의 값이나 상태를 변경하기 위해서 사용

### HTTP의 보안성
* HTTP는 암호화가 되어 있지 않다.
    - 도청 : TCP packet sniffing
        ** 
    - 위증 : 통신 상대를 확인하지 않기 때문
    - 변조 : 정보가 원본인지 모른다.
