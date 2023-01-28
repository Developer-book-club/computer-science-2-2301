# Chapter 2 Network(네트워크)

# Section 2.1 네트워크 기초

## 개론

> 💡 **네트워크**는 컴퓨터 등의 장치들이 통신 기술을 이용하여 구축하는 연결망을 지칭합니다.
>
> 💡 **네트워크**란 노드(node)와 링크(link)가 서로 연결되어 있거나 연결되어 있으며 리소스를 공유하는 집합을 의미합니다.

### 📌 Network(네트워크) 용어 정리하기

- 노드(node)
    - 노드(node)란 서버, 라우터, 스위치 등 **네트워크 장치**
- 링크(link)
    - 링크(link)란 노드(node)와 노드(node)를 연결하는 **물리적인 선로**
- 처리량(throughput)
    - 처리량(throughput)이란 링크(link)를 통해 전달되는 **단위 시간당 데이터 양**
- bps(bits per second)
    - bps(bits per second)란 네트워크의 처리 단위를 나타내며 **1초에 전송 또는 수신되는 비트 수**
- 대역폭(bandwidth)
    - 대역폭(bandwidth)이란 링크(link)를 통해 전달되는 **데이터의 양**
    - 주어진 시간 동안 네트워크 연결을 통해 흐를 수 있는 **최대 비트(bit) 수**
- 지연 시간(latency)
    - 지연 시간(latency)이란 요청이 처리되는 시간, 특정 메시지가 두 장치 사이를 왕복하는 데 걸리는 시간
    - 지연 시간은 매체 타입(무선 or 유선), 패킷 크기, 라우터의 패킷 처리 시간에 영향을 받음
- 신뢰성(reliability)
    - 신뢰성(reliability)이란 데이터가 손실되지 않고 전송되는 능력

## 네트워크 토폴로지(Network Topology)

> 💡 네트워크 토폴로지(Network Topology)란 노드(node)와 링크(link)의 배치된 방식 또는 연결 형태를 의미합니다.
>
구성된 네트워크 토폴로지에 따라 **네트워크 장애 대처법**이 달라지고, **네트워크의 성능**이 달라집니다.  
그렇기 때문에, 처음부터 각 토폴로지들이 어떠한 장단점을 가졌는지 명확하게 이해하고 설계해야 합니다.

### 📌 Network Topology 정리하기

- **버스형(Bus) 토폴로지**
    - **중앙 통신 회선 하나에** 모든 노드가 연결되어 있는 형태
    - 대표적으로 근거리 통신망(LAN)에서 사용합니다.
- **트리형(Tree) 토폴로지 | 계층형 토폴로지**
    - 트리 형태로 네트워크를 구성하였으며, 중앙에는 루트 노드가 존재합니다.
    - 루트 노드에서 각 노드로 향하는 **링크는 하나씩만 존재**합니다.
- **스타형(Star) 토폴로지**
    - 중앙에 있는 루트 노드에 모두 연결되어 있는 형태
    - 루트 노드에 문제가 발생하면 네트워크 전체에 영향을 미칩니다.
- **링형(Ring) 토폴로지**
    - 각각의 노드가 **양옆의 두 노드와 연결**되어 있고, 전체적으로 링 형태로 연결된 형태
    - 데이터의 전달은 노드에서 노드로 이동하게 되어, 노드의 수가 많아질수록 전송 시간이 길어집니다.
- **메시형(Mesh) 토폴로지**
    - 네트워크의 구조가 그물망처럼 구성되어 있고, 하나의 단말에서 장애가 발생하더라도 네트워크를 지속 가능합니다.

### 📌 네트워크 성능 분석 CLI 명령어

- ping(Packet INternet Groper)
  ```text
  ping [대상 호스트명 또는 IP 주소]
  ping www.google.com
  ```
    - ping(Packet INternet Groper)은 네트워크 상태를 확인하기 위해 대상 노드를 향해 패킷을 전송합니다.
- netstat(Network Statistics)
  ```text
  netstat -a
  ```
    - netstat(Network Statistics)는 접속되어 있는 서비스들의 네트워크 상태를 확인하기 위해 사용하는 명령어입니다.
    - -a 옵션은 현재 네트워크 상태를 보여줍니다.
- nslookup(Network Service Lookup)
  ```text
  nslookup
  > google.com
  ```
    - nslookup(Network Service Lookup)은 DNS에 관련된 내용을 확인하기 위해 사용합니다.
    - 특정 도메인(ex: google.com)에 매핑(Mapping)된 IP를 확인하기 위해 사용합니다.
- tacert(Traceroute) | tarceroute
  ```text
  tacert [대상 호스트명 또는 IP 주소]
  tacert www.google.com
  ```
    - tacert(Traceroute)는 목적지 노드까지 네트워크 경로를 확인할 때, 사용합니다.
    - 목적지 노드까지 각 노드에서 패킷을 수신한 시간을 확인합니다.


