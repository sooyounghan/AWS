-----
### Network
-----
1. Local Network
<div align="center">
<img src="https://github.com/user-attachments/assets/15b59bb3-77b3-4bc6-9537-0c82366cfccd" />
</div>

   - 여러 노드의 경로를 찾고 올바르게 전달될 수 있는 기능과 수단을 정의
   - 주요 단위 : Packet
   - 주요 구성 요소 : Router, IP, ARP
   - 주요 특징
     + 서로 떨어진 Local Network 간의 통신 지원 (Network 간의 : Inter Network - Internet)
     + 중간 Node들을 거쳐 목적지까지 도달할 수 있는 방법 지원

2. IP(Internet Protocol) Address
   - Internet Protocol 상에서 통신 주체를 식별하기 위한 아이디
   - 두 가지 종류
     + IPv4 : 32 Bits ($2^{32}$ = 약 43억개) : IP를 최대로 활용하기 위해 사설(Private) IP와 공인(Public) IP로 분류
     + IPv6 : 128 Bits ($2^{64}$)
       * IPv4보다 $2^{96}$배 더 많음
       * 따라서 사설 IP 개념이 불필요함
   - MAC Address와 다르게 수시로 변동 가능

3. CIDR (Classless Inter Domain Routing)
   - IP는 주소 영역을 여러 네트워크 영역으로 나누기 위해 IP를 묶는 방식
   - 여러 개의 사설망을 구축하기 위해 망을 나누는 방법
   - 즉, IP 주소의 집합이며, 네트워크 주소와 호스트 주소로 구성
   - 각 호스트 주소 숫자만큼 IP를 가진 네트워크 망 형성 가능
   - A.B.C.D/E 형식
     + 예) ```10.0.1.0/24, 172.16.0.0/12```
     + A, B, C, D : 네트워크 주소 + 호스트 주소 표시
     + E : 0 ~ 32 값으로, 네트워크 주소가 몇 Bit인지 표시
<div align="center">
<img src="https://github.com/user-attachments/assets/fc5b8143-dbe4-4d5c-9551-60260c43b7c9" />
<img src="https://github.com/user-attachments/assets/24acad5a-e7f3-4ab0-b74e-738df97f1700" />
</div>

4. CIDR Block
   - 호스트 주소 비트만큼 IP 주소 보유 가능
   - 예) ```192.168.2.0/24```
     + 네트워크 비트 : 24
     + 호스트 주소 = 32 - 24 = 8
     + 즉, $2^{8}$ = 256개의 IP 주소 보유
     + ```192.168.2.0 ~ 192.168.2.255```까지 총 256개의 주소 의미

5. Subnet Mask
   - 어느 부분이 호스트 비트인지, 어느 부분이 네트워크 비트인지 구분해주는 Mask
     + AND 연산을 활용해 네트워크 주소를 필터링
     + AND 연산 : 두 비트 중 둘 다 1 일때 만, 1 (예) 1 AND 1 = 1, 0 AND 1 = 0, 1 AND 0 = 0)
   - 네트워크 비트 수만큼 1을 보유한 마스크를 IP에 적용하면 네트워크 비트만 추출 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/57c60f9a-aad0-4150-b2a5-50eb6b8d0795" />
<img src="https://github.com/user-attachments/assets/a9de1fed-f62a-4be3-abe5-591567346c8d" />
</div>

6. Router
   - 네트워크 간 패킷을 주고 받는 Layer 3 장치
   - IP 대역별 최적 경로를 수집 및 관리
     + 어떤 경로의 노드를 경유해야 가장 효율적으로 대상에 도착하는지 알고 있음 (Router Table)
     + 이 경로를 바탕으로 특정 IP 주소가 대상인 패킷의 전달을 요청 받을 때, 해당 경로로 요청

   - 로컬 네트워크는 자신의 로컬 네트워크 주소가 아니라면 라우터로 전달 (확인 방법 : 네트워크 주소가 같은지 확인 (Subnet Mask 등)
   - 이후 Router는 패킷을 Frame 안에 넣어서 최적 경로에 따른 다른 Rotue로 전달 (IP 주소에 따른 Frame 확인 방법 : ARP)

7. ARP (Address Resoultion Protocol)
   - IP로 MAC Address를 찾는 프로토콜
   - 순서
     + Broadcase (MAC Address ```FF:FF:FF:FF:FF:FF```)로 IP 요청
     + 응답 받은 IP MAC Address를 기반으로 MAC 확정 후 테이블에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/eadf8587-6055-46dc-a7ad-8a04d7c71fc4" />
<img src="https://github.com/user-attachments/assets/cefa8014-d65c-4182-82db-e9e15a4843e3" />
<img src="https://github.com/user-attachments/assets/cc728ca0-ce51-437a-b2a0-eb7e299ccf9f" />
</div>

8. 라우터까지 데이터 전달하기
<div align="center">
<img src="https://github.com/user-attachments/assets/c50b5cdf-c696-4c08-aba2-25d8059c4d93" />
<img src="https://github.com/user-attachments/assets/7084a011-28d3-433e-9193-09e1086688f1" />
<img src="https://github.com/user-attachments/assets/a51531d2-b650-4772-b06a-c34309278561" />
</div>

   - IP 대상(```63.12.33.12```)이 로컬이 아닌 것을 식별 : 네트워크 주소가 다름
   - 같은 네트워크가 아니라면 라우터로 전달 : 하지만 라우터의 IP는 알지만, MAC 주소를 모름
   - ARP(Address Resoultion Protocol)을 활용 (IP와 MAC Address를 찾는 프로토콜)
   - 이후 해당 MAC Address로 Frame 생성 후 전달

<div align="center">
<img src="https://github.com/user-attachments/assets/a4a53962-edd4-49a4-be6b-6fb79614440e" />
<img src="https://github.com/user-attachments/assets/c03526e2-b81b-46eb-b699-88fd9e388d91" />
<img src="https://github.com/user-attachments/assets/682d089a-9654-45c5-aa90-e2ffba8546c5" />
<img src="https://github.com/user-attachments/assets/84f11b5d-4067-45bc-88e8-66949324f273" />
<img src="https://github.com/user-attachments/assets/fb40d396-85d8-4c6b-bfdc-c43eb90c0a20" />
<img src="https://github.com/user-attachments/assets/a51e0684-8fb0-4e27-b40b-cceb1ba49fb9" />
<img src="https://github.com/user-attachments/assets/b3ff2f5e-e9a4-40d2-b2d9-ea6a2e5e9040" />
<img src="https://github.com/user-attachments/assets/9d820e6b-9a95-46b8-bd26-b0028afb2bb9" />
</div>

9. Network Layer에서 해결하지 못한 문제
    - 한 번에 하나의 통신만 가능 : 즉, 여러 애플리케이션이 동시에 통신 불가
    - 패킷 등의 순서 보장 / 유실에 대한 대응 불가
  
