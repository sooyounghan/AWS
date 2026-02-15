-----
### AWS SSM Session Manager
-----
1. 인스턴스에 대해 원클릭 액세스를 제공하는 관리형 서비스 : 인스턴스에 SSH 연결 없이, 포트를 열 필요 없이, Bastion Host를 유지할 필요 없이 로그인 가능
2. IAM 유저 단위 제어 가능 (Key 파일로 제어 할 필요 없음)
   - 예) 수백개의 인스턴스에 대해 일일히 로그인을 위한 키 파일을 로그인 해야 할 때
   - 예) 개발자 별 지정된 팀의 인스턴스만 로그인을 할 수 있도록 하고 싶을 때
   - 웹 브라우저 기반으로 OS와 무관하게 사용 가능

3. 로깅과 감사
   - 언제, 어디서, 누가 접속했는지 확인 가능 (CloudTrail)
   - 접속 기록과 사용한 모든 커맨드 및 출력 내역을 S3 혹은 CloudWatch로 전송 가능
   - AWS의 서비스와 연동되어 있어 다양한 시나리오 구현 가능 (예) EventBridge 등과 연동하여 실시간으로 접근에 대한 알림 받기)
<div align="center">
<img src="https://github.com/user-attachments/assets/b933a164-85d5-48a1-9123-7ee415e31067" />
<img src="https://github.com/user-attachments/assets/e2ee1d74-0c0d-453d-9154-ef9da3443460" />
<img src="https://github.com/user-attachments/assets/0c0c9560-c91f-444f-9572-42bd48d0d0a5" />
</div>

4. EC2 Instance Connect Endpoint
<div align="center">
<img src="https://github.com/user-attachments/assets/de7741a8-b161-453e-8fb6-9244b2b0c24c" />
</div>

5. Session Manager
<div align="center">
<img src="https://github.com/user-attachments/assets/55a148f2-d369-4539-a159-ccbc27290dd2" />
</div>

   - 요구사항
     + EC2 Instance에 SSM Agent가 설치되어 있을 것(Amazon Linux 및 여러 공식 AMI에는 기본 설치)
     + EC2 AmazonEC2RoleforSSM(구) / AmazonSSMManagedInstanceCore(신) Managed Policy가 포함된 Role이 적용되어 있을 것
     + SSM Agent가 SSM 및 필요한 서비스에 접근할 수 있을 것 : Private이라면 VPC Endpoint가 해당 VPC에 있을 것
       + ssm
       + ssmmessages
       + ec2messages
       + logs(로깅 활성화 시)
       + s3(s3 로깅 활성화 시)

   - AWS의 구조
<div align="center">
<img src="https://github.com/user-attachments/assets/96da86e2-723c-4e92-9a0d-786a744b319d" />
<img src="https://github.com/user-attachments/assets/c1d106b8-6956-41b3-9884-d82ea0e44ad5" />
</div>

   - Interface Endpoint
<div align="center">
<img src="https://github.com/user-attachments/assets/98340463-0691-428c-b4b2-121a8ce16e2b" />
</div>

   - 💡 Security Group에서 Out-Bound 443포트가 열려있을 것
