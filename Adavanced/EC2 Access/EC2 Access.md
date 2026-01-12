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

2. Instance Connect Endpoint를 활용해 Private EC2 연결
   - EIC Endpoint에 접근 가능할 것
   - 보안 그룹에 22번 포트가 열려 있을 것
