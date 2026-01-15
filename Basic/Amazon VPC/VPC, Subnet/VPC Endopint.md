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
   - S3 및 DynamoDB만 지원
   - 정책 적용 가능 (보안그룹 X)
<div align="center">
<img width="1524" height="685" alt="image" src="https://github.com/user-attachments/assets/52298ce2-14f4-4a2d-95e5-e7c695c4a46f" />
</div>

6. 상시 비용 발생

-----
### 실습 - VPC Endpoint 활용 (도쿄 Region)
-----
1. S3(AWS 파일 관리 서비스) Gateway Endpoint 만들기 : Private Instance에서 S3 데이터 조회
   - VPC - VPC 생성 - my-vpc-vpc
     + VPC 엔드포인트 없음
     + 서브넷 4개 (Public 1개, Private 1개 * 2)
   - IAM 생성
     + 역할 - 역할 생성 - 사용 사례 : EC2
     + AmazonS3FullAccess, AmazonSQSFullAccess
     + 역할 이름 : demo-ec2-role-for-vpc-endpoint
   - EC2 인스턴스 생성
     + demo-my-public-ec2
     + 키 페어 : demo-my-vpc-keypair
     + 네트워크 설정 : 프로젝트-vpc / 서브넷 : public1 선택
     + 퍼블릭 IP 자동 할당 활성화
     + 기존 보안 그룹 선택, 일반 보안 그룹 : default
   - 고급 세부 설정
     + IAM 인스턴스 프로파일 : demo-ec2-fole-for-vpc-endpoint

   - 새로 생성된 보안 그룹에 대해 인바운드, 아웃바운드 트래픽 모두 개방
     + 생성된 VPC의 보안 그룹 확인 후, 인바운드 규칙 삭제 및 추가 (모든 트래픽, 0.0.0.0/0)

   - S3 생성
     + 버킷 만들기 : demo-my-vpc-test-계정ID (버킷 이름은 전세계 글로벌 유니크해야함)

   - EC2 연결 (EC2는 S3로 통신 가능 / SQS로 통신 가능 - 인터넷으로 통해 Public로 접속 가능)
```
sudo -s
aws s3 ls
aws sqs list-queues
```

   - Private EC2 생성 
     + demo-my-private-ec2
     + 키 페어 : demo-my-vpc-keypair
     + VPC : my-vpc-vpc
     + 서브넷 : private1 사용
     + 보안 그룹 : 기존 보안 그룹 선택 (default)
     + 고급 세부 정보 : IAM 인스턴스 프로파일 (demo-ec2-role-for-vpc-endpoint)

   - demo-my-public-ec2 (Bastion Host로 이용) 접속
```
sudo -s
nano keyfile.pem
chmod 400 keyfile.pem
ssh -i "keyfile.pem" ec2-user@private_ip

sudo -s
aws s3 ls // 경로가 없으므로 접속 불가
aws sqs list-queues // 경로가 없으므로 접속 불가
```

   - VPC Endpoint 생성
     + VPC - 엔드포인트 - 엔드포인트 생성
     + Gateway Endpoint : my-s3-gateway-endpoint
     + 유형 : AWS 서비스
     + 서비스 : S3 검색 후 Gateway 유형 선택
     + VPC 선택 : my-vpc-vpc
     + 라우팅 테이블 모두 선택

   - 라우팅 테이블 확인 (Private) : 대상 하나 발생 - 확인하면, S3 대상 라우팅 정보 
     + 예) pl-61a54008 (관리형 접두사 목록 ID) - 접두사 목록 이름 : 	
com.amazonaws.ap-northeast-1.s3 (S3) 

   - EC2에서 확인
```
sudo -s
aws s3 ls // 경로가 생겼으므로 접속 가능
aws sqs list-queues // 경로가 없으므로 접속 불가
```

2. SQS Interface Endpoint 만들기 : Private Instance에서 SQS 데이터 조회
   - SQS 대기열 생성 : my-sqs
   - VPC 엔드포인트 생성
     + my-sqs-endpoint
     + 유형 : AWS 서비스
     + 서비스 : SQS 검색 후 선택
     + VPC 선택 : my-vpc-vpc
     + 서브넷 (어떤 서브넷에 인터페이스 엔드포인트를 만들 것인가 결정)
     + 프라이빗 DNS 이름 활성화 체크 (중요)
     + 보안 그룹 설정 가능
```
sudo -s
aws sqs list-queues // 경로가 생겼으므로 접속 가능
```

3. 리소스 정리
   - VPC Endpoint 삭제
   - EC2 인스턴스 종료
   - VPC 삭제
