-----
### Dead Letter Queue
-----
1. 시스템 오류로 처리할 수 없는 메세지를 임시로 저장하는 특수 유형 메세지 대기열
   - SQS에서 처리 하지 못한 메세지를 담아두어 처리하는 다른 Queue
   - 추후 재처리 가능 (Redrive)

2. 설정한 재시도 횟수보다 많이 실패했을 경우 DLQ로 전달
<div align="center">
<img src="https://github.com/user-attachments/assets/ee6915de-4f35-48e7-a950-fff2595c884d" />
</div>
