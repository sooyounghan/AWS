-----
### Monitoring
-----
1. 기본적으로 CloudTrail로 API 로깅 가능
2. CloudWatch 기본 지표로 다양한 Metric 제공
3. 주요 메트릭
   - ApproximateAgeOfOlderMessage : (근사값) 가장 오래된 메세지의 나이
   - ApproximateNumberOfMessageNotVisible : (근사값) 현재 In-Flight 중인 메세지
   - ApproximateNumberOfMessagesVisible : (근사값) 현재 Stored, 즉, 대기 중인 메세지
   - NumberOfEmptyReceives : 메세지 요청 시 빈 응답이 전달된 횟수
   - NumberOfMessagesReceived : 메세지 요청에 따라 전달된 메세지 갯수
   - NumberOfMessagesSent : Queue에 도착한 메세지 갯수 
   - SentMessageSize : Queue에 도착한 메세지 크기
