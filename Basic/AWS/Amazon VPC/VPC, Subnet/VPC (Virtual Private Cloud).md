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
