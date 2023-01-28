# Chapter 2 Network(네트워크)

# Section 2.4 IP 주소

## 📌 ARP(Address Resolution Protocol)

> 💡 ARP (Address Resolution Protocol)는 IP 주소로부터 MAC 주소를 구하는 **IP**와 **MAC** 주소의 다리 역할을 하는 프로토콜입니다.
>
컴퓨터와 컴퓨터 간의 통신은 1️⃣ IP 주소에서 ARP를 통해 MAC 주소를 찾아 2️⃣ MAC 주소를 기반으로 통신합니다.

- ARP를 통해 가상 주소인 **IP 주소**를 실제 주소인 **MAC 주소**로 변환합니다.
- RARP는 실제 주소인 **MAC 주소**를 가상 주소인 **IP 주소**로 변환합니다.

### 라우팅 테이블(Routing Table)

> 💡 라우팅 테이블 (Routing Table)은 송신지에서 수신지까지 가는 경로를 저장하는 테이블입니다.
> 특정 목적지에 도달하기 위해 거쳐야 할 다음 라우터의 정보와 게이트웨이(Gateway)를 저장합니다.
>

### 게이트웨이(Gateway)

> 💡 게이트웨이 (Gateway)는 서로 다른 통신망, 프로토콜을 사용하는 네트워크의 통신을 가능하게 하는 관문 역할을 하는 장비를 말합니다.
>

### 📒 IP 주소 용어 정리하기

- 라우팅 (Routing)
    - IP 주소를 통해 데이터를 전달하는 과정
- 패킷 (Packet)
    - 데이터를 전달하는 단위
- 패킷 헤더 (Packet Header)
    - 패킷에 대한 정보를 담고 있는 부분

## 📌 IPv4 (Internet Protocol version 4)

> 💡 **IPv4**는 32비트를 8비트 단위로 점을 찍어 표기하며, 각 점단위의 8비트를 숫자로 표현하여 표현합니다.  
> ex) `01111111.0000000.00000000.00000001` -> `127.0.0.1`
>

### DHCP(Dynamic Host Configuration Protocol)

> 💡 DHCP(Dynamic Host Configuration Protocol)는 네트워크 장치의 IP 주소를 수동으로 설정할 필요 없이 인터넷에 접속할 때마다 **자동으로 IP 주소**를 할당할 수 있도록
> 도와주는 **네트워크 관리 프로토콜**입니다.
>

### NAT(Network Address Translation)

> 💡 NAT(Network Address Translation)는 패킷이 라우팅 장치를 통해 전송되는 동안 패킷의 IP 주소를 수정하여, 다른 주소로 매핑하는 방법입니다.
>

- NAT 장치는 대표적으로 사설 IP를 공인 IP로 변환해주는 **게이트웨이** 역할을 합니다.
- NAT는 사설 IP를 사용하는 여러 대의 호스트가 하나의 공인 IP 주소를 이용하여 인터넷에 접속하기 위해 사용합니다.
- NAT는 내부 네트워크의 IP(사설 IP)와 외부 IP(공인 IP)를 다르게 유지할 수 있기 때문에, 어느 정도의 보안이 가능합니다.
- NAT는 **Port(포트) 번호**를 이용하여 내부 네트워크의 호스트를 구분합니다.

