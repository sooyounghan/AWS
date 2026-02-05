-----
### Amazon SNS Topic / Subscription
-----
1. 토픽 (Topic) : SNS의 커뮤니케이션 채널
   - 해당 토픽에 메세지를 발행(Publish)하면, 토픽을 구독(Subscription)한 모든 대상에게 메세지 발생
   - 즉, 하나의 메세지를 다수의 대상이 처리 (= Fan-Out)

2. 구독 프로토콜
   - 이메일
   - HTTP(S)
   - SQS
   - SMS
   - Lambda
   - Kinesis Data Firehose

3. 최초 구독 시 확인 필요 (예) 확인 이메일, Lambda의 경우 확인 이벤트)
