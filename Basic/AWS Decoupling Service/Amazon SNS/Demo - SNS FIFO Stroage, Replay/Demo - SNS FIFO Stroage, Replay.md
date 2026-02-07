-----
### Demo - SNS FIFO Stroage / Replay
-----
1. SNS FIFO와 SQS 큐로 연결하는 Subscription 생성
   - DynamonDB에 요청을 저장하는 로직
   - 이메일로 보내는 로직

2. 데이터를 삭제하고 Replay로 DB에 데이터가 다시 생성되는 것 확인
3. S3에 데이터를 저장하는 로직을 담은 Subscription 추가 : 이후 Replay로 기존 로직과 Sync을 맞추는 부분 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/e1ca6cca-6591-4208-bcbc-df8636dc550a" />
<img src="https://github.com/user-attachments/assets/f258c608-5647-45e8-82c1-3b6752c1a555" />
<img src="https://github.com/user-attachments/assets/4e78f807-3d72-4522-860a-c1378cd6e590" />
<img src="https://github.com/user-attachments/assets/222ddee1-2c7b-4072-9eea-2a55b3a8efe3" />
<img src="https://github.com/user-attachments/assets/e59b9fea-dea4-4fbf-a839-362be2cc9718" />
<img src="https://github.com/user-attachments/assets/df978139-fbfd-409c-b60f-872dc1ec380c" />
</div>
