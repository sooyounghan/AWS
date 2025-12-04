-----
### Session 
-----
1. OSI 7 Model과 TCP / IP Model
<div align="center">
<img src="https://github.com/user-attachments/assets/bb0ca5ef-f09a-4b12-b4c4-e047d584ec53" />
<img src="https://github.com/user-attachments/assets/d7fa4637-ef3d-497e-8b5e-02d6bed7341f" />
</div>

2. Session Layer
   - 통신 주체끼리 연결을 유지할 수 있는 방법 정의
   - 예전 컴퓨팅 환경에서 Layer 1, 2, 3, 4 이외의 차원에서 지속적 연결(세션)이 수립될 수 있는 방법 제공 (예) MAC Address(2)와 IP(3) 주소와 포트(4)가 동일한 상황에서 유저를 구분하는 방법은 무엇인가?
   - 현재도 마찬가지로 Layer 4 이상의 추가적 차원에서 지속적 연결(세션)을 수립할 수 있는 방법을 포함 (예) HTTP Cookie)
<div align="center">
<img src="https://github.com/user-attachments/assets/48d78ad0-fdaf-4610-8ca2-261360e219a8" />
</div>

   - 일부 프로토콜의 경우 Session Layer 자체를 구현하지 않음 (예) FTP)

3. HTTP와 FTP
   - HTTP
<div align="center">
<img src="https://github.com/user-attachments/assets/b9e69f56-e57b-45a3-b43e-e37097ead045" />
</div>


   - FTP
<div align="center">
<img src="https://github.com/user-attachments/assets/718541b5-ff27-4e70-8f9a-9746d3e62517" />
</div>

4. Transport / Session Layer
   - 통신 주체끼리 데이터 전달의 신뢰성을 확보하는 방법 정의
   - 주요 단위 : 세그먼트
   - 주요 구성 요소 : TCP / UDP
   - 주요 특징 : Network Layer로 성립된 통신 위에서 실질적인 활용을 위한 다양한 기능 정의 (패킷 순서 보장, 에러 처리, 포트 기반 분할 등)

5. TCP (Transmission Control Protocol)
   - 패킷의 전달 과정에서 순서를 보장하고 유실되지 않도록 보장할 수 있는 통신 규약 : 패킷 안에 세그먼트를 담아 주고 받아서 로직 처리
   - 연결 지향 : 즉, 지속적으로 채널을 수립하여 전달 여부를 확인하고 무결성 확인
     + 단, 지속적으로 무결성을 확보하는 과정에서 비교적 느리고 복잡한 과정 필요
   - 주요 사용 사례 : 웹 페이지(HTTP / HTTPS), 이메일, 파일 전송, SSH 등

6. 세그먼트 (Segment)
   - TCP의 데이터 전달 단위
   - 주요 구성
     + Port (Source / Destination)
     + Sequece / Acknowledgement Number : 통신 주체끼리 데이터를 주고 받았는지 확인에 사용
     + Flags : Segment의 목적 등 정의 (예) ACK, SYN, FIN)
     + Window Size : 세그먼트를 만든 주체가 얼마나 많은 데이터를 받을지 전달
     + Urgent Pointer : 세그먼트의 중요도를 설정 (먼저 처리해야 하는 세그먼트 등)
     + 기타 : Checksum 등
     + 실제 데이터

   - TCP 세그먼트
<div align="center">
<img src="https://github.com/user-attachments/assets/4d88390b-1ff4-4d85-a78c-1d37d75b1fcf" />
</div>

   - Layer 3
<div align="center">
<img src="https://github.com/user-attachments/assets/bafd0f03-955d-4039-a203-03545f4e2420" />
</div>

   - TCP
<div align="center">
<img src="https://github.com/user-attachments/assets/911a6587-b935-426d-8b9b-fa42ecfee11b" />
<img src="https://github.com/user-attachments/assets/57aa69d5-654a-47e3-99f8-20ccafb02b8b" />
<img src="https://github.com/user-attachments/assets/5c763b1d-bfc0-4be6-851b-9bf70f0e0c45" />
<img src="https://github.com/user-attachments/assets/f9c1ba17-f8e9-48e1-99ee-73927842e594" />
</div>

7. Port
   - IP 프로토콜에서 패킷을 올바른 프로세스로 라우팅 하기 위한 논리적 단위
   - TCP Port / UDP Port로 구분 : 각 0 ~ 65535까지 정수 범위
<div align="center">
<img src="https://github.com/user-attachments/assets/e798ea56-f065-4c50-a71a-64c73288268f" />
</div>

   - Well Known Port : 주로 서버에서 사용하는 애플리케이션 / 프로토콜 별로 미리 지정된 포트로 주요 사용에 따라 공통적으로 사용
     + 예) 80 : HTTP, 443 : HTTPS, 22 : SSH, 3036 : MySQL
   - Ephemeral Port : 클라이언트에서 사용하는 포트로 연결할 때 임의로 지정
<div align="center">
<img src="https://github.com/user-attachments/assets/cfca41fe-b53e-4975-a934-c4c9ddfb1504" />
</div>

8. TCP Handshake
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

9. User Datagram Protocol (UDP)
    - 빠르게 간단하게 데이터를 주고 받을 수 있는 방법 정의
    - Conntectless
      + 연결 지향과는 달리 데이터의 무결성, 순서, 전달여부를 확인하지 않음
      + 즉, 패킷이 순서대로 오지 않거나, 중복되거나, 아예 전달되지 않을 수 있음
    - 주요 사용 사례 : 보이스톡, 스트리밍, 온라인 게임

10. UDP Segment
<div align="center">
<img src="https://github.com/user-attachments/assets/0f8478a3-83a5-493c-a9ee-3067d911effe" />
</div>

11. 주요 프로토콜 (TCP / UDP)
    - TCP
      + HTTP / HTTPS : TCP / 80, 443
      + FTP (File Transfer Protocol) : TCP / 20, 21
      + SSH (Secure Shell) : TCP / 22
      + DNS (Domain Name System) TCP : TCP / 53

    - UDP
      + DNS (Domain Name System) : UDP / 53
      + DHCP (Dynamic Host Configuration Protocol) : UDP / 67, 68
      + VoIP (Voice over IP) : UDP / 5060

