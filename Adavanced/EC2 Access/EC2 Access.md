-----
### EC2 접속 방법
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/b86f361c-b7aa-4f90-9f34-a589605d8460" />
</div>

-----
### Systems Manager Session Manager
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/b5d679d2-b38b-4cf3-a181-1da1d955be0c" />
<img src="https://github.com/user-attachments/assets/ec53669a-dde7-4f74-b86f-8f90b1a7ccc5" />
</div>

-----
### EC2 Instance Connect Endpoint
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/3bf52025-4341-47f2-b5a0-7b7f62fdc474" />
<img src="https://github.com/user-attachments/assets/b91adcb0-d4a2-4d71-8888-64fc6ecf9b9e" />
</div>

-----
### 비교
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/e2b98743-708f-460d-a0d5-a388d6282d72" />
</div>

-----
### Demo
-----
1. SSM Session Manager를 활용해 Private EC2 연결
   - 요구사항
     + EC2 Instance에 SSM Agent가 설치되어 있을 것 (Amazon Linux에서는 기본 설치)
     + EC2 AmazonEC2RoleforSSM Managed Policy가 포함된 Role이 적용되어 있을 것
     + VPC Endpoint가 해당 VPC에 있을 것
       * ssm
       * ssmmessages
       * ec2messages
       * logs(로깅 활성화 시)
       * s3(s3 로깅 활성화 시)

     + Security Group에서 Outbound 443 포트가 열려있을 것

   - IAM 역할 생성
     + 역할 생성
       * EC2 - AmazonSSMManagedInstanceCore
       * demo-ec2-role-for-ssm
       * 만들어진 역할에 권한 추가 - 정책 연결 - CloudWatchAgentServerPolicy (CloudWatch 로그 남기거나 전달 권한 존재)

    - EC2
      + demo-my-private-ec2
      + 키 페어 없이 계속 (키 페어를 사용해서 SSH로 로그인 하지 않기 때문임)
      + 네트워크 : my-vpc-vpc
      + private 서브넷 선택
      + 기존 보안 그룹 선택 - default
      + 고급 세부 정보 : IAM 인스턴스 프로파일 (demo-ec2-role-for-ssm)

    - SSM Session Manager를 위한 VPC Endpoint 프로비전 (하나의 가용 영역에서만 동작)
      + VPC - 엔드포인트
      + my-ssm-endpoint / 서비스 : com.amazonaws.ap-northeast-1.ssm / VPC : my-vpc-vpc / 서브넷 : 가용 영역 1개 선택 후 private과 연결 / 보안 그룹 default
      + my-ssm-messages-endpoint / 서비스 : com.amazonaws.ap-northeast-1.ssmmessages / VPC : my-vpc-vpc / 서브넷 : 가용 영역 1개 선택 후 private과 연결 / 보안 그룹 default
      + my-ec2-messages-vpc-endpoint / 서비스 : com.amazonaws.ap-northeast-1.ec2 / VPC : my-vpc-vpc / 서브넷 : 가용 영역 1개 선택 후 private과 연결 / 보안 그룹 default
      + my-cloudwatch-endpoint / 서비스 : com.amazonaws.ap-northeast-1.logs / VPC : my-vpc-vpc / 서브넷 : 가용 영역 1개 선택 후 private과 연결 / 보안 그룹 default

    - AWS Systems Manager
      + 세션 관리자 - 세션 시작 - test - 대상 인스턴스 : demo-my-private-ec2
      + Start session
      + 확인 : sudo -s / dir
      + 기본 설정
        * S3 로깅
        * CloudWatch 로깅
          * CloudWatch - 로그 그룹 생성 - demo-my-ssm-log 
          * CloudWatch 로깅 - 활성화 - 암호화 적용 해제 - 목록에서 로그 그룹 선택에서 위 로그 그룹 체크
          * 세션 관리자 - 세션 시작 - log - 대상 인스턴스 : demo-my-private-ec2
          * CloudWatch - 로그 그룹 - 로그 스트림 확인

   - 리소스 정리 : Endpoint 삭제 
2. Instance Connect Endpoint를 활용해 Private EC2 연결
   - EIC Endpoint에 접근 가능할 것
   - 보안 그룹에 22번 포트가 열려 있을 것
   - VPC 엔드포인트 생성
     + EC2 인스턴스 연결 엔드포인트
     + VPC : my-vpc-vpc
     + 보안 그룹 : default
     + 서브넷 지정 : private2
   - EC2 인스턴스 연결
     + 연결 유형 : 프라이빗 IP를 사용하여 연결
   - 리소스 정리 : 
