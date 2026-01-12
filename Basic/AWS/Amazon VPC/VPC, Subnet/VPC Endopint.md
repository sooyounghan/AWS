-----
### Virtual Private Cloud
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/1e1c190b-fe20-4eec-b43a-1676843b3d3a" />
<img src="https://github.com/user-attachments/assets/7a925474-35df-4139-91ea-0144e9af5ae4" />
<img src="https://github.com/user-attachments/assets/9ba46d9d-c623-4da3-94e6-63aa1cbee51a" />
</div>

-----
### VPC Endpoint
-----
1. 인터넷 게이트웨이, NAT 디바이스, VPN 연결 또는 AWS Direct Connect 연결을 필요로 하지 않고, AWS PrivateLink 구동 지원, AWS 서비스 및 VPC 엔드포인트 서비스에 비공개로 연결할 수 있음
2. VPC 인스턴스는 서비스의 리소스와 통신하는데 Public IP 주소를 필요로 하지 않음 : VPC와 기타 서비스 간 트래픽은 Amazon 네트워크를 벗어나지 않음
3. 외부 인터넷을 거치지 않고 AWS 서비스에 연결시켜주는 리소스
4. Interface Endpoint : ENI(Elastic Network Interface) 기반
   - Private IP를 만들어 서비스로 연결
   - 많은 서비스들을 지원 (SQS, SNS, Kinesis, SageMaker 등)
   - 서브넷 지정 필요
   - 보안 그룹 / 정책을 통해 보호 가능
<div align="center">
<img width="1514" height="686" alt="image" src="https://github.com/user-attachments/assets/07b60cc5-7e6c-49f7-927a-a2784591fe05" />
</div>

5. Gateway Endpoint : 라우팅 테이블에서 경로의 대상을 지정하여 사용
   - S3 및 DynamoDB 지원
   - 정책 적용 가능 (보안그룹 X)
<div align="center">
<img width="1524" height="685" alt="image" src="https://github.com/user-attachments/assets/52298ce2-14f4-4a2d-95e5-e7c695c4a46f" />
</div>

6. 상시 비용 발생

-----
### 실습 - VPC Endpoint 활용
-----
1. S3 Gateway Endpoint 만들기 : Private Instance에서 S3 데이터 조회
2. SQS Interface Endpoint 만들기 : Private Instance에서 SQS 데이터 조회
