-----
### Virtual Private Cloud(VPC) 구조
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/5f90f7e4-6f0f-4126-8f4b-0b0aae120415" />
</div>

-----
### NAT Gateway
-----
1. Amazon Virtual Private Cloud(Amazon VPC)의 프라이빗 서브넷에 있는 인스턴스에서 인터넷에 쉽게 연결할 수 있도록 지원하는 가용성이 높은 AWS 관리형 서비스
2. NAT Gateway / NAT Instance
   - 프라이빗 서브넷의 리소스가 외부의 인터넷과 통신하기 위한 통로
   - NAT Instance는 단일 EC2 인스턴스 / NAT Gateway는 AWS에서 제공하는 서비스 : NAT Gateway는 고가용성이 확보된 관리형 서비스
   - NAT Gateway / Instance는 모두 서브넷 단위
     + Public Subnet에 있어야 함
     + 고가용성 확보를 위해서는 두 개 이상 가용영역(서브넷) 필요
   - 비용 발생
     + $0.059 / 시간 (서울 Region 기준)
     + $0.045 / 1GB 트래픽 처리
<div align="center">
<img src="https://github.com/user-attachments/assets/a65878ad-14a0-4b73-aabf-ca6a3bb44d73" />
</div>
