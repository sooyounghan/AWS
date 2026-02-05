-----
### Amazon SNS Security
-----
1. Access Policy : 어떤 주체가 SNS에 접근하여 메세지를 보내거나 구독할 수 있는 리소스 기반 정책
   - 주체에게 권한이 없더라도 권한 부여 가능
   - 예) IAM 사용자에 아무 권한이 없더라도 Access 정책에 권한이 명시되어 있으면 SNS로 Publish 가능

2. 암호화
   - KMS를 활용해 Server Side Encryption 구현
   - Body만 암호화 (Metadata, Timestamp 등은 암호화 처리하지 않음)
