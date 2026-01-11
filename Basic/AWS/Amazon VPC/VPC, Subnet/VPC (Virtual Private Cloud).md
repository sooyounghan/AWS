-----
### AWS 구조
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/c425e3c7-000a-40ae-93b7-10cb2b1e7986" />
<img src="https://github.com/user-attachments/assets/727706b4-de5a-4665-a066-b63853f05879" />
</div>

-----
### VPC (Virtual Private Cloud)
-----
1. 사용자의 AWS 계정 전용 가상 네트워크
2. AWS 클라우드에서 다른 가상 네트워크와 논리적으로 분리되어 있음
3. Amazon EC2 인스턴스와 같은 AWS 리소스를 VPC에서 실행할 수 있으며, IP 주소 범위와 VPC 범위를 설정하고 서브넷을 추가하고, 보안 그룹을 연결한 다음 라우팅 테이블을 구성
4. 즉, 가상으로 존재하는 데이터 센터
   - 원하는 대로 사설망을 구축 가능 : 부여된 IP 대역을 분할하여 사용 가능
   - Region 단위
6. 구조
<div align="center">
<img src="https://github.com/user-attachments/assets/fb388946-06c4-4bf7-9742-49daf6ee0e84" />
</div>

7. VPC 사용 사례
   - EC2, RDS, Lambda 등의 AWS의 컴퓨팅 서비스 실행
   - 다양한 서브넷 구성
   - 보안 설정(P Block, 인터넷에 노출되지 않는 EC2 등 구성)

8. 구성 요소
   - 서브넷
   - 인터넷 게이트웨이
   - NACL / 보안그룹
   - 라우트 테이블
   - NAT Instance / NAT Gateway
   - Bastion Host
   - VPC Endpoint  

-----
### VPC Router
-----
1. VPC에 있는 가상 라우터로 서브넷에서 오고가는 트래픽을 라우팅 : 즉, 모든 서브넷의 트래픽은 VPC 라우터를 거쳐서 목적지에 도달
2. VPC 생성 시 자동으로 생성되며, 별도로 관리할 필요가 없음 : 별도 설정은 불가능하며, Router Table만 관리 가능

-----
### Rotue Table
-----
1. VPC 라우터에서 트래픽이 어디로 가야할지 알려주는 이정표
2. VPC 생성 시 기본으로 하나 제공
3. 구성 요소
   - Destination : 트래픽이 가고자 하는 주소
   - Target : 트래픽을 실제로 보내줄 대상 (논리적 리소스 아이디로 표현[예) Internet Gateway의 경우 IGW-xxxxxx)
<div align="center">
<img src="https://github.com/user-attachments/assets/61811df8-80aa-4537-b39b-26b48a0c98ab" />
<img src="https://github.com/user-attachments/assets/7b421e4d-1dd6-4de9-87ef-10b23443ba4c" />
<img src="https://github.com/user-attachments/assets/ae586a4a-4487-48d0-9235-15468d8470b5" />
<img src="https://github.com/user-attachments/assets/1f1da2e8-c8b8-42f6-bf1d-57cebfe2e402" />
</div>

