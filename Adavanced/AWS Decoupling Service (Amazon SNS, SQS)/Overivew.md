-----
### 디커플링 (Decoupling)
-----
1. 아키텍쳐에서 주체간의 의존성을 낮추는 방법
2. 아키텍쳐의 유연성을 높이고 효율적으로 최적화

-----
### 긴밀한 결합 (Tight Coupling) / 느슨한 결합 (Loose Coupling)
-----
1. 긴밀한 결합
   - 다른 주체에 대해서 단단하게 얽힌 상태
   - 주체끼리 높은 의존성을 가지고 있어 변경이 쉽지 않음
   - 예) 우리 몸

2. 느슨한 결합
   - 다른 요소에 얽히지 않고 연결되어 있는 상태
   - 주체끼리 낮은 의존성을 가지고 있어 쉽게 변경할 수 있고 유연함
   - 예) 전동 공구, 로봇 등
<div align="center">
<img src="https://github.com/user-attachments/assets/cd4876ae-4a11-4ec5-baf8-3e0288882b98" />
</div>

-----
### AWS Decoupling Service
-----
: AWS에서 아키텍쳐에서 유연성을 확보하기 위한 서비스
   - Amazon SQS
   - Amazon SNS
   - Amazon Kinesis
   - Amazon MQ
   - Amazon MSK (Kafka)
   - 기타 다양한 이벤트 기반 기능들 (예) S3 Event, EventBridge, DynamoDB Stream 등)
