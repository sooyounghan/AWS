-----
### Origin Access Control (OAC)
-----
1. OAI(Origin Access Identity : 예전 방식, OAC 권장)와 함께 CloudFront를 거치지 않은 S3에 접근을 방지하기 위한 기능
2. 일종의 Identity : IAM 사용자 혹은 IAM 역할과 비슷한 Identity
   - 즉, S3에서 해당 OAC 접근을 허용하고 CloudFront에서 OAC를 활용해서 S3와 소통
   - S3에서 기본적으로 모든 접근을 차단하고 OAC의 접근만 허용
3. OAC는 Lambda Function URL에도 사용 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/146ed5da-5bf0-493d-8afc-7d4f14d97757" />
</div>

4. 세 가지 Sign 방법 : CloudFront가 S3와 소통하기 위한 요청에 Sign 방법을 정의 가능
   - Sign Requests : CloudFront IAM Principle이 S3에 요청할 때 SigV4 (Signature V4)로 Sign
     + 즉, 요청에 자격증명을 활용해 필요한 정보로 Authorization Header를 구성하고, S3에서 해당 내용을 검증해서 자격이 있는지 요청인지 확인 후 요청 처리 또는 거부
     + 클라이언트가 Sign한 헤더가 있다면 덮어씌움

   - Do not override authorization header : 클라이언트 Header가 있다면 사용, 없으면 새로 Sign
   - Do not sign requests : Authorization Header를 사용하지 않음 (클라이언트가 항상 Sign을 통해 요청하거나, 컨텐츠가 Public인 경우)

-----
### Demo -CloudFront OAC 설정
-----
1. 버킷 생성 : demo-cf-oac-bucket-{계정ID} 
    - flower.jpg 업로드
    - 원본 : S3 버킷 이름
    - 원본 도메인 존재
    - 편집 - 원본 액세스 제어 설정 (권장) / OAC 생성 및 설정 가능
    - 권한 - 설정을 하지 않더라도, CloudFront가 권한 설정 (OAC 생성 및 설정 가능)

2. CloudFront Distribution 생성 (OAC 설정)
    - 배포 생성 : demo-oac-test
    - Origin Type : Amazon S3 / 버킷명 조회 / Grant CloudFront access tot Origin (OAC) 설정
    - 보안 보호 비활성화
    - 배포 DNS 복사 후, 웹에 입력 후 전송하면 정상 그림 출력
      + 버킷 권한 삭제 후 캐싱 무효화 한 뒤, 다시 요청하면 Access Denied 발생 (버킷 정책이 없으므로)
      
