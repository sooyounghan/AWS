-----
### Amazon SNS
-----
1. 문자 메세지 발송 : 도쿄 리전 또는 버지니아 북부 리전에만 한정
2. 주제 - 주제 생성 - 표준 / demo-my-email-test
3. 구독 생성
   - 주제 ARN : demo-my-email-test
   - 프로토콜 : 이메일
   - 엔드포인트 : 이메일 주소 (발송된 이메일 Confirm subscription 확인해야 적용)

4. 주제 - 메세지 개시 - my test email / this is a test message (이메일로 전송)
5. 구독 생성
   - 주제 ARN : demo-my-email-test
   - 프로토콜 : 이메일-JSON
   - 엔드포인트 : 이메일 주소 (발송된 이메일 Confirm subscription 확인해야 적용) - JSON으로 도착
   - 주제 - 메세지 개시 : 동일

6. 구독 삭제 이후, 주제 삭제
