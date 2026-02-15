-----
### VPC Peering
-----
1. VPC 피어링 연결은 프라이빗 IPv4 또는 IPv6 주소를 사용하여 두 VPC 간에 트래픽을 라우팅할 수 있도록 하기 위한 두 VPC 사이 네트워킹 연결
2. 동일한 네트워크에 속하는 경우와 같이 VPC의 인스턴스가 서로 통신할 수 있음
3. 즉, 두 개의 다른 VPC를 연결하여 VPC 안의 리소스끼리 통신이 가능하도록 설정하는 것
4. 💡 다른 리전, 다른 계정의 VPC끼리 연결 가능
5. CIDR Range의 중첩 불가능
<div align="center">
<img src="https://github.com/user-attachments/assets/6c1f0683-b913-4635-a481-ff60986c72e4" />
</div>

6. Transitive Peering 불가능
   - 통신을 위해서는 직접 연결 또는 Transit Gateway 이용
<div align="center">
<img src="https://github.com/user-attachments/assets/f26fc65e-1a2d-4078-abad-81f9001bb7c7" />
<img src="https://github.com/user-attachments/assets/66294579-3ebf-4957-ab10-fdfab2caeed5" />
<img src="https://github.com/user-attachments/assets/86d142d2-076b-4f94-b70b-cb34bf171172" />
</div>

7. AWS Transit Gateway
   - 중앙 허브를 통해 VPC들과 온프레미스 네트워크를 연결하는 서비스
   - 복잡한 Peering 관계를 제거하여 간소화
   - Region 간에는 서로 다른 Transit Gateway 끼리 연결

8. 💡 Peering 설정 이후 라우팅 테이블을 업데이트해서 경로 설정 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/0d481d9b-36d6-4d67-9fa8-a79688b3abc0" />
</div>

-----
### AWS VPN (Virtual Private Network)
-----
1. 온프레미스 네트워크, 원격 사무실, 클라이언트 디바이스 및 AWS 글로벌 네트워크 사이에서 보안 연결 설정
2. Site-to-Site VPN
   - 데이터센터와 AWS를 연결하는 하드웨어-하드웨어 연결 (1:1 연결)
   - IPSec 프로토콜 사용

3. Client VPN
   - AWS와 온프레미스 / AWS VPN 장비와 사용자의 PC를 연결하는 하드웨어-소프트웨어 연결 (1:N)
   - PC가 네트워크 내부에 있는 것 처럼 작동
   - TLS 프로토콜 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/eccfb310-1c53-45fe-8f34-6033bbab7a01" />
</div>

-----
### AWS Direct Connect
-----
1. AWS 리소스에 대한 최단 경로
2. AWS와 온프레미스 간에 DX Location을 경유한 전용선을 통해 연결
3. 외부 인터넷과 분리되어 있음
   - 외부에서 접근이 불가능하기 떄문에 높은 보안
   - 인터넷 환경에 상관 없이 일정한 속도 보장
4. Direct Connect Gateway : 여러 리전의 VPC를 한 번에 Direct Connect로 연결할 수 있는 서비스
5. 성능이 좋고 빠른 대신 비싸고 설치에 오래 걸림
<div align="center">
<img src="https://github.com/user-attachments/assets/3d0b8e95-c3f7-4ed1-8833-fa44e9280f00" />
<img wsrc="https://github.com/user-attachments/assets/146c770c-f359-4e4b-88fb-4fcc40df4d10" />
</div>

-----
### Amazon EFS
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/794ae849-6d8c-4eb6-bdf8-6a0761018f39" />
</div>

-----
### Demo - VPC Peering (도쿄 리전)
-----
1. VPC 1과 VPC 2를 연결
   - VPC 생성 (VPC 등)
     + my-vpc-1 / 10.0.0.0/16 / VPC 엔드포인트 없음
     + my-vpc-2 / 10.1.0.0/16 / VPC 엔드포인트 없음 
   - VPC Peering 연결
     + 피어링 연결 - 피어링 연결 생성 - VPC ID 설정 - 계정 : 내 계정 - my-vpc-2
     + 작업 - 요청 수락
     + DNS - DNS 설정 편집 - 요청자 / 수락자 DNS 확인 - 변경 사항 저장
   - 라우팅 테이블에 연결 추가
     + Public Subnet 연결 (my-vpc-1-rtb-public에서 my-vpc-2-rtb-public)
     + my-vpc-1-rtb-public 선택 - 라우팅 - 라우팅 편집
       * 10.1.0.0/16
       * 피어링 연결
       * my-vpc-peering

     + my-vpc-2-rtb-public 선택 - 라우팅 - 라우팅 편집
       * 10.0.0.0/16
       * 피어링 연결
       * my-vpc-peering

3. VPC 1의 EC2에서 VPC 2의 EC2에 Private IP로 연결
    - demo-vpc-1
      + 네트워크 설정 - VPC : my-vpc-1-vpc / my-vpc-public1 서브넷 설정 / 퍼블릭 IP 자동 할당 활성화
      + 기존 보안 그룹 선택 : default

    - demo-vpc-2
      + 네트워크 설정 - VPC : my-vpc-2-vpc / my-vpc-public2 서브넷 설정 / 퍼블릭 IP 자동 할당 활성화
      + 기존 보안 그룹 선택 : default

    - 보안 그룹 설정 (💡 해당 EC2 보안 그룹만 설정 - 기존 default는 건드리지 말 것)
      + 인바운드 규칙 - 기존 삭제 후, 모든 트래픽 / 0.0.0.0/0
    - demo-vpc-2 접속
```
sudo -s
dnf install httpd -y
service httpd start
```
   - demo-vpc-2의 Private IP를 통해 demo-vpc-1에서 확인
```
sudo -s
curl {demo-vpc-2의 Private IP}
```

4. 리소스 정리 : EC2 종료, VPC 피어링 삭제(관련 라우팅 테이블 항목 삭제), VPC 삭제
