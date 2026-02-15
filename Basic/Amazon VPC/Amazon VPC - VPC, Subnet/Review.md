-----
### 정리
-----
1. VPC (Virtual Prviate Cloud)
   - 가상의 데이터 센터
   - AWS 계정 생성 시 기본 VPC와 기본 서브넷이 생성 : 기본 서브넷은 Public Subnet
   - Region 단위

2. 서브넷 (Subnet)
   - VPC 하위 단위로 VPC에 할당된 IP를 더 작은 단위로 분할한 개념
   - 하나의 서브넷은 하나의 가용영역(AZ)안에 위치
   - Public Subnet : IGW를 통해 인터넷과 연결되어 있는 서브넷
   - Private Subnet : 인터넷과 연결되어 있지 않은 서브넷
   - CIDR Notation으로 IP 주소 지정
<div align="center">
<img src="https://github.com/user-attachments/assets/73e1089e-2060-42cb-8b32-3a5f701584ff" />
</div>

3. Amazon EFS
<div align="center">
<img src="https://github.com/user-attachments/assets/8c1265b7-1805-4e62-bd51-4f231e6de82f" />
</div>
