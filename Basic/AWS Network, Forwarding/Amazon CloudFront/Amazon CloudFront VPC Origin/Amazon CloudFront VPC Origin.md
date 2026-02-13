-----
### Amazon CloudFront VPC Origin
-----
1. Amazon CloudFront VPC Origins
   - CloudFront의 Origin(원본)으로 VPC 안의 리소스를 지정할 수 있는 기능
   - ALB / NLB / EC2 지원
   - 보안그룹으로 트래픽 통제 가능 : CloudFront Managed Prefix 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/c184117b-e154-4d02-8238-7cb4131270ee" />
<img src="https://github.com/user-attachments/assets/24449905-f519-4fe9-9926-cd27087aaede" />
<img src="https://github.com/user-attachments/assets/6167f9cd-e5fb-4df3-9e2f-409832bef084" />
<img src="https://github.com/user-attachments/assets/67afc769-e99c-48e1-8a08-e6e45014e17e" />
<img src="https://github.com/user-attachments/assets/e7630be9-00c7-4fe6-ad05-57ec18f2c386" />
</div>

2. 제약 사항
   - CloudFront와 같은 계정의 VPC만 연동 가능
   - 💡 지원하는 리전의 VPC만 사용 가능 : AP-Northeast-2의 경우 apne2-az1 사용 불가 (ALB에 포함되어 있어도 불가능)
<div align="center">
<img src="https://github.com/user-attachments/assets/856fd865-e482-42d1-af20-5f283a5d188c" />
</div>

   - VPC에 Internet Gatweay 필요 (인터넷 통신용이 아니므로 대상으로 Route Table 연동 필요 업음)
   - Private Subnet에 적어도 하나의 IPv4 주소 가용 필요 (ENI 확보용)
   - 웹 소켓, gRPC, Origin Rewrite with Lambda@Edge, Response Timeout, Keep Alive Timout 불가능

3. Demo - VPC Origin
   - CloudFormation으로 VPC + Internal ALB + Private EC2 프로비전
     + CloudFormation - 스택 생성 - 템플릿 파일 업로드 - 스택 이름 : demo-cf-vpc-origin - IAM 리소스 승인
     + 리소스 프로비전 완료

   - CloudFront VPC Origin으로 해당 컨텐츠 제공
     + EC2 인스턴스 확인 : 내부 EC2, LB이므로 확인 불가하므로 CloudShell 확인
     + ```+```으로 Create VPC Enviornment - my-vpc-shell / VPC : demo-cf-vpc-origin / Private Subet 1  / 보안 그룹은 생성된 보안 그룹 선택 - Create
     + curl 로드밸런서DNS이름 : 인스턴스 ID 출력
     + CloudFront - VPC 오리진 - 생성
       * demo-vpc-origin
       * 오리진 ARN : Elastic Load Balancer 선택
       * 프로토콜 : HTTP만 해당
       * 배포 - 배포 생성 - demo-my-vpc-origin - VPC 오리진 - demo-vpc-origin 선택 - 도메인 네임 복사 후 접속하면 EC2 ID 정상 출력

   - 리소스 정리 : CloudFront 배포 비활성화 / CloudFormation Template 삭제 / CloudFront 배포 삭제 / VPC 오리진 삭제
