-----
### Demo - SQS FIFO vs Standard
-----
1. SQS Standard와 SQS FIFO를 만들고 로컬 애플리케이션에서 실험
   - 각각 순서 보장 / 중복 여부
   - 각각 Visibility Timeout이 만료되면 Queue의 어디로 돌아가는지 확인
   - 각각 Visibility Timeout이 만료되고 메세지를 삭제하면 어떻게 되는지 확인

2. 정리
<div align="center">
<img src="https://github.com/user-attachments/assets/7f31ad6d-2d97-4bdd-9190-64289732a39c" />
</div>

3. SQS
   - demo-my-standard-quque : 표시 제한 시간 2초
   - demo-my-fifo-queue.fifo : FIFO 선택 (표시 제한 시간 2초)
   - IAM - 사용자 - admin - 보안 자격 증명 - 기타 - 액세스 키 만들기
```
aws configure --profile awsclassroom
```
   - .env 
```
STAGE=dev
VER=1
REGION=ap-northeast-2
QUEUE_URL=demo-my-standard-quque
FIFO_QUEUE_URL=demo-my-fifo-queue.fifo
AWS_PROFILE=awsclassroom
```

   - 디버그 - StandRefillMessages
```
{QueueUrl: 'https://sqs.ap-northeast-2.amazonaws.com/362454057659/demo-my-standard-quque', MessageBody: '{"counter":0}'}
StandardRefillMessages.js:24
Success, message sent. Counter: 0
```
   - StandardConsumeTest
```
Message received: 0 ,delta: 0
StandardConsumeTest.js:41
No messages in the queue
StandardConsumeTest.js:34
Duplicated messages: 0  Total Message: 1 ,Totlal Percentage of duplicated messages: 0 % ,Max Delta: 0
```

   - 메세지 1000개를 넣고, Consume : Standard Queue는 순서 보장이 잘 되지 않음
```
Duplicated messages: 0  Total Message: 1000 ,Totlal Percentage of duplicated messages: 0 % ,Max Delta: 216
```

   - FifoRefillMessage : 100개로 설정 후, Consume
   - Visibility Timeout 만료 시, Queue의 어디로 돌아가는지 확인
```
StandardVisiblityTimeoutTest : 10개를 넣고 테스트 (3초 대기 후 실시, [2초로 설정했으므로])
- 랜덤하게 나오므로, 어디로 돌아가는지 알 수 없음 (짐작 : 맨 뒤)
Message received: {"counter":1}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":0}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":2}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":2}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":0}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":8}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":0}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":2}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":8}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":7}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":6}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":3}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":8}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":1}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":7}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":6}
StandardVisiblityTimeoutTest.js:27
Message Returened to Queue
StandardVisiblityTimeoutTest.js:30
Message received: {"counter":7}
....
```
```
FIFOVisiblityTimeoutTest : 동일하게 10개로 테스트
- 큐의 맨 뒤로 돌아감 (순서가 아예 변경)
Message received: {"counter":0}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":1}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":2}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":3}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":4}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":5}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":6}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":7}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":8}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":9}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":0}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":1}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":2}
FIFOVisiblityTimeoutTest.js:28
Message Returened to Queue
FIFOVisiblityTimeoutTest.js:32
Message received: {"counter":3}
...
```

  - 💡 큐 비우기 : Consume 또는 콘솔에서 메세지 대기열 삭제 (demo-my-fifo-queue.fifo)
    + 삭제 : 대기열(큐) 자체를 제거
    + 제거 : 정답 (대기열에 있는 메세지 제거)
    + 다른 방법 : 대기열 - demo-my-fifo-queue.fifo 선택 후 작업 - 제거

  - Visbility Timeout이 끝난 메세지를 지우려고 할 때
```
StandardLateDeleteTest : : 메세지 1개 설정
- 삭제 가능
```
```
FIFOLateDeleteTest : 메세지 1개 설정
- 메세지가 지워졌으므로, 에러 발생 (요청도 실패)
```

4. 액세스 키 삭
