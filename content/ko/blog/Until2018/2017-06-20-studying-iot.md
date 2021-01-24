---
title: "studying IOT"
date: 2017-06-20

---

#Internet
*internetworking 기술: 여러가지 네트워크를 연결하여 하나의 기준을 둠으로서, 다양한 하드웨어 기술이 결할될 수 있도록 만드는 기술*

## TCP/IP 네트워크 모델
**특징**
 - 특정 하드웨어나 운영체제에 독립적인 개방형 프로토콜
 - 물리적 네트웨크에 독립적.
 - 전세계에 유일한 주소체계를 사용한다.
 - 표준화되어있으며 널리 사용된다.
**동작과정**
  - TCP는 사용자의 데이터를 TCP 패킷으로 나눈다.
  - IP는 TCP 패킷에 목적지 주소에 대한 정보를 추가한다.
  - TCP는 각각 도착한 IP패킷을 모아 호스트 컴퓨터가 사용할 수 있는 데이터로 변환, 변환시 오류 검출, 오류 발생시 재전송을 요청한다.

서비스 제공을 위한 연결
HTTP : 데이터(메세지)
TCP : 세그멘트(TCP헤더 + 데이터)
IP : 패킷(IP헤더 + 세그멘트)
이더넷 : 프레임(패킷 + 이더넷 헤더)
네트워크 인터페이스: 비트스트림

*하드웨어를 통한 연결*
**인터넷은 LAN(Local Area Network)를 기반으로 구성한다.**
  - 물리계층과 데이터계층이 하드웨어에 따라 다르다.
  - 이더넷 - IEEE802.3
  - 와이파이 - IEEE802.11
**이더넷이나 와이파이로 연결된 기기들은 48비트의 MAC 주소로 구분한다**

**IP 주소**
v4는 32비트에 인데 반해 v6는 128비트이다.
지역별로 할당되어 데이터 전달 경로를 결정하기에 적합하도록 만들어진 소프트웨어적 주소.
ARP 프로토콜을 통해 MAC주소와 IP주소를 매핑한다.

IP: 패킷으로 만들어진 데이터를 목적지까지 전달하는 과정을 담당한다.
TCP: 데이터가 목적지까지 '정확하게' 전달될 수 있도록 책임진다.
  - 두 기기사이의 데이터 흐름을 관리
  - 수신된 데이터에 대해서 acknowledgement를 통해 데이터가 수신되었음을 알려준다.

*UDP vs TCP*
UDP는 비연결형, 단순한 전송 프로토콜로서, 신뢰성이 없고 ack가 없는 best-effort 전송 방식이다. 재전송은 수행하지 않고, 데이터 흐름 관리 기능이 없다. 대신 부하가 매우 낮고, 전송 속도가 매우 빠르다. 데이터의 완전성보다 전달 속도가 중요하고, 소량의 데이터를 송신하고, 멀티캐스트/브로드캐스를 사용하는 어플리케이션에 사용된다.데이터의 일부가 손실되는 것이 중요하지 않은 어플리케이션(비디오, 멀티미디어 스트리밍)에 쓰인다.

TCP는 전송 전 연결을 먼저 맺어야 하는 연결형이며, 어플리케이션이 네트워크 계층 문제를 걱정하지 않고 데이터를 안정적으로 송신할 수 있도록 하는 풍부한 기능의 프로토콜이다. 메세지 전송을 신뢰할 수 있고, 모든 데이터에 대한 ack이 존재하여 손실된 데이터는 자동으로 재전송한다. 윈도우 크기를 적절히 조정하며 슬라이딩 윈도우를 이용한 흐름제어를 하고, 혼잡 회피 알고리즘을 사용하여 데이터 흐름 관리를 한다. 부하, 전송속도 모두 우수하지만, udp보다는 낮다. 소형부터 초대형 데이터까지 송신가능하며, 신뢰할 수 있는 방법으로 데이터를 송신해야하는 대부분의 프로토콜과 어플리케이션. 대부분의 파일/메세지 전송 프로토콜을 포함한다. TCP/IP 어플리케이션을 위한 전송 계층 프로토콜. HTTP, FTP, SMTP등에 사용한다.

> TCP는 데이터 전송시 데이터를 세그멘트로 분할하여 **순서대로** 전송한다. 단, ack를 받지 못한 세그멘트에 대해서는 재전송한다. 목적지의 host는 sequence number에 따라 데이터를 재구성한다.

*인터넷 연결을 위한 설정*

고정 ip
유동 ip
 - DHCP(Dynamic Host Configuration Protocol)
사설 ip
 - 공식적으로 IP 주소 사용에 대한 허가를 받지 않고 임의로 사용
 - 192.168.xxx.xxx 가 많이 사용된다.
서브넷 마스크
 - 하나의 ip 네트워크 주소를 다시 여러 ip 서브 네트워크로 분할하는 기능을 수행.
브로드캐스트 주소
 - ip와 subnetmask를 and 연산하여 얻어진 네트워크 주소에서 subnetmask의 0으로된 비트를 모두 1로 바꾸어주면 구할 수 있다.  -> 네트워크: 165.132.120.0 서브넷: 255.255.252.0 일때 브캐: 165.132.123.255이다.
게이트웨이
 - 인터넷과 직접 연결되어 있는 주소
DNS
 - 호스트 네임을 ip 주소로 변환한다. 윈도우에서 ping, nslookup, tracert를 이용하면 ip 확인이 가능하다.
*네트워크를 효율적으로 나누기 위해 서브넷을 이용하는 것이 유용하나, 다수의 ip 주소가 낭비된다 (네트워크 id, 브로드캐스트 주소등)*


**이더넷**
이더넷 쉴드
- 근거리 네트워크(Local Area Network, LAN)을 구성하는 방식에 관한 표준
- 1983년 IEEE 802.3으로 표준화
- 유선 연결을 통한 네트워크의 90% 이상을 차지

이더넷 라이브러리
- ipaddress 클래스, ethernetclass 클래스, Ethernetclient 클래스, Ethernetserver 클래스 등으로 구성.
- ipaddress 클래스 : ip주소를 지정하기 위한 클래스
* ipv4를 지원.

- ethernetclass 클래스 : 아두이노가 이더넷을 통해 데이터를 주고 받을 수 있도록 이더넷 쉴드를 설정하는 역할. 이더넷 라이브러리와 네트워크 설정을 초기화, ip주소를 동적으로 할당받을 수 있도록 dhcp 지원. but, dhcp 사용시, 스케치 크기 증가.
* int begin(u8int_t mac_address)
*int maintain()함수는 dhcp를 통해 할당된 ip주소를 갱신할 때 사용한다. 0-4까지 반환값에 따라, renew(동일 dhcp서버에서 ip 갱신) rebind(새로운 dhcp 서버로부터 주소 할당) 성공 여부를 나타낸다.*

- Ethernetserver 클래스: 다른 컴퓨터나 기기에서 실행 중인 클라이언트의 요청을 받아들이고 클라이언트와의 통신을 관리하기 위한 클래스.
 * Ethernetserver(uint16_t port) : 지정한 포트로 들어오는 클라이언트의 요청을 감시하는 서버 객체를 생성.
 * void begin(void): 서버를 시작하여 서버 생성시에 지정한 포트로 들어오는 클라이언트 요청을 감시한다. 클라이언트의 요청이 들어오면 클라이언트와 통신하기 위한 소켓을 생성하여 클라이언트와 연결. 이더넷은 최대 4개의 소켓을 동시에 사용.
 * Ethernetclient available(void) : 서버에 접속된 클라이언트 중 서버에서 읽어 들일 수 있는 데이터를 보낸 클라이언트 객체를 반환. 클라이언트의 접속 요청이 있을 때, 클라리언트의 접속을 허용하는 절차를 Ethernet 라이브러리가 자동으로 처리하여 연결이 이루어지면, 서버는 클라이언트 별로 연결을 관리한다.
 * size_t write(const uint8_t \*buf, size_t size) : 서버에 접속된 모든 클라이언트로 데이터 전송, 개별 클라이언트로 전송된 바이트 수를 합한 값이 반환됨.
 * print : serial 클래스의 print와 동일.
- Ethernetclient 클래스
 * Ethernetclient() : 이더넷 클라이언트 객체를 생성하는 생성자로 생성된 클라이언트는 connect 함수를 통한 지정한 ip주소와 포트를 가진 서버로 접속한다.
 * int connect : 지정된 서버의 주소와 포트를 통해 서버로 접속을 시도한다. 성공시 true 실패시 false를 반환한다. 서버의 주소값은 문자열일 수도 있고 IPAddress객체의 형식일수도 있다.
 * int read : 수신버퍼의 바이트 데이터를 읽는다
 * void flush : 서버로부터 전송되어 아직 읽지 않은 데이터를 읽어서 수신버퍼를 비운다.
 * void stop : 접속 종료


# Iot를 위한 프로토콜

**HTTP : Hypertext transfer protocol**
 - Request/Response 프로토콜 (client가 서버에 정보 요청. 서버가 응답)
 - Request : Method, resource, headers, Content
 - Response : statusCode, Headers, Content

* Method
 - Get, Put/Delete, Post등
* HTTP protocol stack

*HTTP/HTTPS(URLs)*

*TCP(port# default: 80/443)*

*Internet Protocol(IP, ip주소들)*

*Local Area Network(LAN, mac 주소들)*

*Physical(Cables, Radio, etc)*

*SSL/TLS로 암호화하면 https*

**CoAP : Constrained Application Protocol**
 - 메모리, 에너지, 성능 등에 제약이 있는 M2M 환경을 위한 웹 기반 프로토콜.
 - UDP 기반의 Request/Response 모델로 동작.
 - UDP와 CoAP 계층 사이에 DTLS가 사용될 수 있음.
 - binary header가 더 간결하고, option의 수가 감소, method도 4가지만 존재.

HTTP의 tcp 대신 udp(port 5683)이 들어간다는게 차이점.

**MQTT : Message Queue Telemetry Transport protocol**
 - Publisher, Subscriber, Message broker, Topics
 - Http와 다르게, TCP 포트가 1883/8883(보안 무/유)으로 지정되어있다.
 - 3 QoS

# Wifi

**와이파이는 이더넷과 더불어 인터넷에 물리적으로 연결되기 위 한 방법을 제공**
 - 실제 데이터 교환과 서비스 제공은 상위 계층에서 이루어짐
 - 물리적인 연결의 차이점을 제외하면 이더넷과 거의 동일한 방법으로 와이파이 사용이 가능

**CSMA/CA**

A가 B로 패킷을 보내고 싶을 때, B에게 먼저 RTS를 보낸다. B가 RTS를 받으면, A에게 CTS를 보냄으로써 응답한다. C가 CTS를 overhear하면, keeps quiet for a while.

A가 B에서 RTS보내면 A주변의 노드들이 NAV(무선통신에서 사용하는 virtual carrier sensing mechanism)를 통해 keep quiet하게 된다. B에서 A로 CTS 보낼 떄 B 주변의 노드들 또한 NAV를 통해 keep quiet하게 된다.

**Wifi 라이브러리**

- Wifi, Wificlient, WifiServer 클래스 정의.
- 이더넷과 차이는 네트워크 연결을 위해 AP와의 연결이 필요(AP의 SSID와 패스워드가 필요할것입니다).

*Wifi 클래스 (전체적인 와이파이 연결시작, 신호세기, 맥주소 할당등 연결을 위해 필요한 설정을 담당한다)*
- begin 함수는 연결하고자 하는 와이파이의 ssid, password를 파라미터로 전달합니다. config함수와 달리 dhcp를 통해 유동 ip를 자동으로 할당받는다. config함수는 정적 ip 주소를 사용하기 때문에 파라미터에 IPAddress 객체를 전달해야한다.

*Wifi Server 클래스*
 - 이더넷과 흡사하게 available함수는 서버에 접속된 WifiClient 객체를 반환한다.
*Wifi Client 클래스*
 - 이더넷과 마찬가지로, connect 함수를 사용하여 지정된 ip주소와 포트를 통해 서버에 접속한다.
