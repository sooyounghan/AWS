-----
### Producer
-----
1. 메세지는 API를 사용해 Push 가능 : 이 때, Queue URL을 알고 있어야 정확한 SQS Queue를 판별하여 메세지 전송 가능
2. Queue URL 형식 : ```https://sqs.<region>.amazonaws.com/<account-id>/<queue-name>```
   - 예) ```https://sqs.us-east-1.amazonaws.com/123456789012/MyQueue```
3. IAM 엔티티로서 권한이 있거나 리소스 기반 정책으로 권한 확보 필요 (sqs:SendMessage)
4. 다양한 AWS 서비스에 이벤트로 연동 : 기본적으로 연동 가능하거나 EventBridge로 연동
<div align="center">
<img src="https://github.com/user-attachments/assets/da9b5fa9-03ef-4f4d-b2ba-3a2a94dcbbcd" />
</div>

