-----
### SQS / SNS FIFO - Message Group ID
-----
1. Message Group ID : SNS / SQS FIFO 내부의 일종의 채널
2. Message Group ID 단위로 순서 보장 및 전달이 이루어짐
   - 💡 즉, 다른 Message Group ID 끼리는 순서 보장이 이루어지지 않음
   - SQS FIFO에서는 동일 Message Group ID를 가진 메세지는 동시에 하나만 처리 가능 : 하나의 Message Group에서 맨 처음 메세지가 처리되지 않으면 나머지 Message Group 모두 대기

3. SNS FIFO에서 Message Group ID를 전달했을 때, 대상이 SQS FIFO라면 Message Group ID 같이 전달
<div align="center">
<img src="https://github.com/user-attachments/assets/c63e3351-8205-4702-b99f-46b8545c2c59" />
<img src="https://github.com/user-attachments/assets/2e9b1886-9605-4d49-a578-ef4a0892d9d7" />
</div>
