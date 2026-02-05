-----
### Amazon SNS Message 필터링
-----
1. 메세지 필터링을 통해 모든 메세지 대신 특정 메세지만 수신 가능
2. 각 Subscription Filter Policy 설정
   - Filter Policy : 메세지의 Body 혹은 Attributes 단위로 원하는 메세지 매칭
   - 매칭 시 메세지 발송
   - 불일치 시 메세지를 전달하지 않음

3. 필터링 가능 대상 : Message Body, Message Attributes (예시) 구매 타입='Express' AND (구매 나라가 미국 OR 유럽) AND 가격이 20불 이상)
<div align="center">
<img src="https://github.com/user-attachments/assets/ceb39395-40cf-4df8-b1f0-ec86f82319b2" />
<img src="https://github.com/user-attachments/assets/05178d45-4ace-48fe-8d23-12c9eef8ca1a" />
</div>

