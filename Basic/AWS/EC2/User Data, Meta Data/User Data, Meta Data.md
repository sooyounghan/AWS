-----
### EC2 User Data
-----
1. EC2 구성
<div align="center">
<img src="https://github.com/user-attachments/assets/f4aa1a36-9b1a-45f9-925f-4e289f9e3982" />
</div>

2. EC2 User Data
   - EC2 인스턴스 최초 실행 시 지정한 스크립트로 실행 가능 : 별도 설정을 통해 재부팅마다 실행하도록 설정 가능
   - 두 가지 모드
     + Shell Script
     + cloud-init : Linux 이미지의 Boot-Straping을 위한 Open Source Application

   - 주요 사용 사례
     + EC2 인스턴스 설정 (보안 설정, 인스턴스 설정 등)
     + 외부 패키지 다운로드
     + 설치 애플리케이션 실행
     + 기타 EC2 실행 시 필요한 동작
<div align="center">
<img src="https://github.com/user-attachments/assets/e61d3b56-4051-4eff-b2a1-b2e6f6e0ca4f" />
</div>


3. 예제 시나리오 - 인스턴스 등록
<div align="center">
<img src="https://github.com/user-attachments/assets/2eda5a40-1a5f-4902-a571-be6345ae1a75" />
<img src="https://github.com/user-attachments/assets/f14f8c11-c13e-4458-8395-57940e254fbf" />
<img src="https://github.com/user-attachments/assets/40c3483d-25a9-43a8-8a38-a719f0eaf58a" />
<img src="https://github.com/user-attachments/assets/041e5546-679e-4122-be40-8b3356b474b7" />
</div>

4. Amazon EC2 Instance Metadata
   - 인스턴스를 구성 또는 관리하는데 사용될 수 있는 인스턴스 관련 데이터
   - 호스트 이름, 이벤트 및 보안 그룹과 같은 범주로 분류
   - EC2 인스턴스의 속성 및 정보 데이터 : AMI ID, IPv4 / IPv6, EBS 맵핑, 보안 그룹 연동상황, IAM 역할 연동 등
   - 실행 중 EC2 인스턴스의 메타데이터 IMDS(Instance Metadata Service)로 조회 가능
     + HTTP Endpoint 지원
     + IP 주소 : ```169.254.169.254(IPv4), fd00:ec2::254(IPv6)```
     + 두 가지 모드
       * IMDS v1 : Request / Response 기반
       * IMDS v2 : Session 기반 (Default)
   - 주요 사용 사례 : 인스턴스 별 설정, IAM 임시 자격증명 조회 등 (AWS CLI, SDK 등 내부적 활용)
   - EC2 실행 시 메타데이터 액세스 가능 여부 설정 가능
     + 기본 활성화
     + 버전 선택 가능
   - 가격 : 무료
   - 참고로 Tag 조회의 경우 별도로 활성화해야 Metadata로 Tag 조회 가능 : 활성화 하지 않을 경우 목록에서 보이지 않음

5. IMDS(Instance Metadata Service) V1
   - 별도의 보안 인증이 필요 없는 Request / Response 기반 : Link Local IP(```169.254.169.254```)를 사용하므로 해당 EC2 인스턴스에서만 요청 가능
   - CloudWatch Metric을 활용해 조회 횟수 기록 가능
   - 예시) 인스턴스 이름 가져오기 : ```curl http://169.254.169.254/latest/meta-data/tags/instance/Name```

6. IMDS(Instance Metadata Service) V2
   - 보안 토큰을 발급받아 요청할 때 마다 토큰을 사용해 인증하는 세션 방식 : 토큰의 유효기간은 1초에서 최대 6시간
   - IMDSv1보다 더 높은 보안 수준 제공 : IAM 정책 등 활용해 EC2 인스턴스가 IMDS V2를 사용하도록 강제 가능
<img width="745" height="544" alt="image" src="https://github.com/user-attachments/assets/73a3f92b-5777-4e56-ac87-7f0b5d050d03" />

   - 예시
     + ```TOKEN=`url-X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` ```
     + ```curl -H "X-aws-ec2-metada-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/tags/instance/Name```

7. Demo - User Data / Metadata 활용
   - EC2가 올라갈 때 웹 서버(Apache) 설치 및 기동
   - 이 과정에서 index.html에 자신의 인스턴스 아이디 업데이트 : Meta Data를 활용해 자신의 인스턴스 아이디 조회해 index.html에 업데이트
