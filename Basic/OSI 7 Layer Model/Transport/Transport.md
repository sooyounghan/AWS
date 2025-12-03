-----
### Transport
-----
1. 상황
   - Network Layer - Packet
<div align="center">
<img src="https://github.com/user-attachments/assets/ebbf9704-d6ea-4f15-b43c-1f927cc38d0e" />
</div>

   - Data Layer - Frame
<div align="center">
<img src="https://github.com/user-attachments/assets/94a649c0-6576-42bf-9d60-b826bac87617" />
<img src="https://github.com/user-attachments/assets/6e238b96-1fa6-4673-b6d5-843b8a863621" />
<img src="https://github.com/user-attachments/assets/1dcf9184-f9fa-4c3a-a469-c7ae52f00d1a" />
<img src="https://github.com/user-attachments/assets/22283de0-5f3b-4e70-aeaf-a65ae4de5143" />
<img src="https://github.com/user-attachments/assets/877c99a6-9d61-4d4c-929c-18c961d99d1c" />
<img src="https://github.com/user-attachments/assets/aee354a1-4e7a-4c8d-9696-3715d9356008" />
</div>

   - Network Layer - Packet
<div align="center">
<img src="https://github.com/user-attachments/assets/70400cfc-fc5b-4cfa-9ef3-ee148c8a4b51" />
</div>

2. Transport Layer
   - 통신 주체끼리 데이터 전달의 신뢰성을 확보하는 방법 정의
   - 주요 단위 : Segment
   - 주요 구성 요소 : TCP / UDP
   - 주요 특징 : Network Layer로 성립된 통신 위에서 실질적인 활용을 위한 다양한 기능 정의 (패킷의 순서 보장, 에러 처리, 포트 기반 분할 등)

3. Transport Control Protocol (TCP)
   - 패킷의 전달 과정에서 순서를 보장하고, 유실되지 않도록 보장할 수 있는 통신 규약 : 패킷 안에 세그먼트를 담아 주고 받아서 로직 처리
   - 연결 지향
     + 지속적으로 채널을 수립해 전달 여부를 확인하고 무결성 확인
     + 지속적으로 무결성을 확보하는 과정에서 비교적 느리고 복잡한 과정 필요
   - 주요 사용 사례 : 웹 페이지 (HTTP / HTTPS), 이메일, 파일 전송, SSH 등

4. Segment (세그먼트)
   - TCP / UDP의 데이터 전송 단위
   - TCP 세그먼트의 주요 구성
     + Port (Source / Destination)
     + Sequence / Acknowledgement Number : 통신 주체끼리 데이터를 주고 받았는지 확인에 사용
     + Flags : Segment의 목적 등 정의 (예) ACK, SYN, FIN)
     + Window Size : 세그먼트를 만든 주체가 얼마나 많은 데이터를 받을지 전달
     + Urgent Pointer : 세그먼트의 중요도를 설정 (먼저 처리해야 하는 세그먼트 등)
     + 기타 : Checksum 등)
     + 실제 데이터
<div align="center">
<img src="https://github.com/user-attachments/assets/23cc286d-7ab3-441d-aed9-c97e9434be3f" />
</div>

5. Layer 3
<div align="center">
<img src="https://github.com/user-attachments/assets/d03258bc-4c98-4344-ac1a-26339de72105" />
</div>

6. TCP
<div align="center">
<img src="https://github.com/user-attachments/assets/7582b36d-bfa8-4b0f-8477-89deece4049e" />
<img src="https://github.com/user-attachments/assets/931e32a3-a781-4e10-90a7-b7830de46eea" />
<img src="https://github.com/user-attachments/assets/1a8f4a33-abab-460d-baa7-bcdf8679ae26" />
<img src="https://github.com/user-attachments/assets/95556d5a-e7b2-4d19-95f7-805e0fdb55c2" />
<img src="https://github.com/user-attachments/assets/39dae11f-9b80-4aac-987f-457cdae214f8" />
<img src="https://github.com/user-attachments/assets/bc5087b2-bd5e-42e5-a68c-f5b1d796b625" />
<img src="https://github.com/user-attachments/assets/d40be765-4321-4db3-b6e0-2dc392bc52c9" />
<img src="https://github.com/user-attachments/assets/7bdbb3b6-39ce-4720-8263-e95cbb7d9bec" />
</div>

7. Port
   - IP 프로토콜에서 패킷을 올바른 프로세스로 라우팅 하기 위한 논리적 단위
   - TCP Port / UDP Port로 구분 : 각 0 ~ 65535까지 정수 범위
<div align="center">
<img src="https://github.com/user-attachments/assets/e5008e8d-0f1b-40a7-9c90-1d57be3ebca0" />
</div>

   - Well-Known Port : 주로 서버에서 사용하는 애플리케이션 / 프로토콜 별로 미리 지정된 Port
     + 주요 사용에 따라 표준으로 공통적 사용
     + 예) 80 - HTTP, 443 - HTTPS, 22 - SSH, 3306 - MySQL
   - Ephermeral Port : 클라이언트에서 사용하는 Port로 연결할 때 마다 임의로 지정
<div align="center">
<img src="https://github.com/user-attachments/assets/99d4409d-58ca-40e0-9825-3ea802b82225" />
</div>

8. Stateful, Stateless
<div align="center">
<img src="https://github.com/user-attachments/assets/666da323-1150-4919-acba-3d8c11e75a35" />
</div>

9. TCP Handshake
   - TCP Protocol에서 통신을 수립하고 서로를 인식하는 첫 과정
   - 보통 3-Way Handshake라고 부르며, 3가지 과정으로 구분
     + SYN (Synchronize) : 첫 요청으로 사용할 첫 클라이언트 Sequence Number(CS) 전달
     + SYN - ACK (Synchronize - Acknowledge) : SYNC에 대한 응답으로 CS + 1과 서버 Sequence Number (SS)를 전달
     + ACK (Acknowledge) : 마지막 단계로 연결이 수립되었음을 알려주며 CS + 1과 SS + 1을 전달
<div align="center">
<img src="https://github.com/user-attachments/assets/162e7e7b-2a98-46c4-8930-5ac5597d3a4f" />
<img src="https://github.com/user-attachments/assets/35bb7c86-0fa3-42dd-8163-66553b4d1609" />
<img src="https://github.com/user-attachments/assets/7af6c69c-6e30-4583-8cbb-2932b36d4fb2" />
</div>

10. User Datagram Protocol (UDP)
    - 빠르게 간단하게 데이터를 주고 받을 수 있는 방법 정의
    - Conntectless
      + 연결 지향과는 달리 데이터의 무결성, 순서, 전달여부를 확인하지 않음
      + 즉, 패킷이 순서대로 오지 않거나, 중복되거나, 아예 전달되지 않을 수 있음
    - 주요 사용 사례 : 보이스톡, 스트리밍, 온라인 게임

11. UDP Segment
<div align="center">
<img src="https://github.com/user-attachments/assets/0f8478a3-83a5-493c-a9ee-3067d911effe" />
<img src="https://github.com/user-attachments/assets/8d763d3b-cf3c-462f-ac67-81b8c02635b3" />
<img src="https://github.com/user-attachments/assets/217afad7-2650-4733-af9d-15e3bae4919f" />
<img src="https://github.com/user-attachments/assets/8ab272f7-3b2f-487a-bb9a-bca695c59f72" />
<img src="https://github.com/user-attachments/assets/3b9e472d-f00f-4955-81a3-a28e39b2c43e" />
<img src="https://github.com/user-attachments/assets/67cdc3ee-75e8-42f6-b688-b0f63f77e6c0" />
</div>

12. 주요 프로토콜 (TCP / UDP)
    - TCP
      + HTTP / HTTPS : TCP / 80, 443
      + FTP (File Transfer Protocol) : TCP / 20, 21
      + SSH (Secure Shell) : TCP / 22
      + DNS (Domain Name System) TCP : TCP / 53

    - UDP
      + DNS (Domain Name System) : UDP / 53
      + DHCP (Dynamic Host Configuration Protocol) : UDP / 67, 68
      + VoIP (Voice over IP) : UPD / 5060

13. Network Load Balancer
<div align="center">
<img src="https://github.com/user-attachments/assets/a808f7c1-2c50-4d52-a85a-ca4c346170a4" />
</div>

14. OSI 7 Layer
<div align="center">
<img src="https://github.com/user-attachments/assets/1933801c-4fe3-4ce0-9e30-0f48f4666b07" />
</div>
