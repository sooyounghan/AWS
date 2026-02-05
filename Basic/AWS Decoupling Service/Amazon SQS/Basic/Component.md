-----
### Amazon SQS의 주요 구성 요소
-----
1. 메세지 : SQS에서 전달하는 데이터 단위
2. Producer : 메세지를 생산하고 SQS에 전달하는 주체
3. Queue : 메세지를 저장하고 메세지를 Consumer에게 전달하는 다양한 기능들을 담당
4. Consumer : 메세지를 받아  처리하고 소비(삭제) 하는 주체
5. Dead Letter Queue : 처리에 실패한 메세지를 모아둔 2차 Queue
6. 액세스 정책 : SQS에 접근할 수 있는 주체에 대한 권한 설정 정책
<div align="center">
</div>
<img src="https://github.com/user-attachments/assets/b8b80cb4-9d0d-4492-83a2-82deb3475a21" />

-----
### SQS를 활용하는 디커플링
-----
<div align="center">
<img  src="https://github.com/user-attachments/assets/ed3e97b9-f56e-4260-874a-ecbbe99dff1b" />
<img src="https://github.com/user-attachments/assets/58452a4e-a7d2-4c0a-b1c8-7e42c211a744" />
<img src="https://github.com/user-attachments/assets/dcd74b08-1bd4-4cce-9886-a956d8b364ba" />
<img src="https://github.com/user-attachments/assets/88976db6-5448-4f16-8b3d-92f7cb01dbd6" />
<img src="https://github.com/user-attachments/assets/706f283f-ea33-413c-be72-b16c390e8a68" />
<img src="https://github.com/user-attachments/assets/74c4b873-388a-4e53-a446-e1529f7b11b2" />
</div>
