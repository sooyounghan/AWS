-----
### Amazon SQS 보안
-----
1. Access Policy : 어떤 주체가 SQS Queue에 접근하여 메세지를 보내거나 가져올 수 있는 리소스 기반 정책
   - 주체에게 권한이 없더라도 권한 부여 가능
   - 예) IAM 사용자에 아무 권한이 없더라도 Access 정책에 권한이 명시 되어 있다면, Queue 사용 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/531d29ac-df58-48c7-a8f7-ee68a3ac256a" />
</div>

2. IAM 정책 종류
   - Identity-Based Policies (자격 증명 기반 정책)
     + 자격증명(IAM 유저, 그룹, 역할)에 부여하는 정책
     + 해당 자격 증명이 무엇을 할 수 있는지 허용

   - Resource-Based Policies (리소스 기반 정책)
     + 리소스 (예) S3, SQS, VPC Endpoint, KMS 등)에 부여한느 정책
     + 해당 리소스에 누가, 무엇을 할 수 있는지 허용 가능 (예) SQS 대기열에 Lambda Service가 접근 가능)

3. 암호화
   - SQS-SSE(Encryption At Rest), SQS-KMS 지원 (S3과 거의 동일)
   - Body만 암호화(Metadata, Timestamp 등은 암호화 처리하지 않음)
   
