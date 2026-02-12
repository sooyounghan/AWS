-----
### Origin Access Control (OAC)
-----
1. OAI(Origin Access Identity : 예전 방식, OAC 권장)와 함께 CloudFront를 거치지 않은 S3에 접근을 방지하기 위한 기능
2. 일종의 Identity : IAM 사용자 혹은 IAM 역할과 비슷한 Identity
   - 즉, S3에서 해당 OAC 접근을 허용하고 CloudFront에서 OAC를 활용해서 S3와 소통
   - S3에서 기본적으로 모든 접근을 차단하고 OAC의 접근만 허용
3. OAC는 Lambda Function URL에도 사용 가능
<img width="528" height="274" alt="image" src="https://github.com/user-attachments/assets/146ed5da-5bf0-493d-8afc-7d4f14d97757" />

4. 세 가지 Sign 방법 : CloudFront가 S3와 소통하기 위한 요청에 Sign 방법을 정의 가능
   - Sign Requests : CloudFront IAM Principle이 S3에 요청할 때 SigV4로 Sign
     + 즉, 요청에 자격증명을 활용해 필요한 정보로 Authorization Header를 구성하고, S3에서 해당 내용을 검증해서 자격이 있는지 요청인지 확인 후 요청 처리 또는 거부
     + 클라이언트가 Sign한 헤더가 있다면 덮어씌움

   - Do not override authorization header : 클라이언트 Header가 있다면 사용, 없으면 새로 Sign
   - Do not sign requests : Authorization Header를 사용하지 않음 (클라이언트가 항상 Sign을 통해 요청하거나, 컨텐츠가 Public인 경우)

-----
### Demo -CloudFront OAC 설정
-----
1. 버킷 생성
2. CloudFront Distribution 생성 (OAC 설정)

