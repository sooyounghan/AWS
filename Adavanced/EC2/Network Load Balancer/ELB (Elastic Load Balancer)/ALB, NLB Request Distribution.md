-----
### ALB의 요청 분배
-----
1. Round Robin : 순차적으로 모든 타겟을 돌아가면서 분배
<div align="center">
<img src="https://github.com/user-attachments/assets/07b6b3cc-adaf-4e42-a2b8-8f01f3bff616" />
</div>

2. Least Outstanding Request : 현재 가장 적은 요청을 받고 있는 쪽으로 분배 (엄청난 트래픽이 몰릴 경우 제대로 동작하지 않을 수 있음)
<div align="center">
<img src="https://github.com/user-attachments/assets/99e2cb25-578d-40a1-97bb-480b68f3efc5" />
</div>

3. Weighted Random : 무작위 타겟에 요청을 분배
   - Anomaly Mitigation 활용 가능 : 타겟의 오류 응답에 따라 확률 조정
<div align="center">
<img src="https://github.com/user-attachments/assets/0f024bb5-3681-4dcf-a654-adf95a77fef9" />
<img src="https://github.com/user-attachments/assets/5612f323-5bb4-489e-b0a9-d2d119212483" />
<img src="https://github.com/user-attachments/assets/0e76dca4-816f-4219-b8f2-ccc2a86e885e" />
</div>

-----
### NLB의 요청 분배
-----
1. 요청의 정보를 기반으로 HASH 값을 만들어 타겟 그룹에 분배
2. TCP 6 Tuple : SRC IP, SRC PORT, DEST IP, DEST PORT, Sequence Number, Protocol, SYN Flag
<div align="center">
<img src="https://github.com/user-attachments/assets/72b77e3f-b124-42f1-81e1-d94e6d4188f1" />-----
</div>

3. UDP 5 Tuple : SRC IP, SRC PORT, DEST IP, DEST PORT, Protocol
<div align="center">
<img src="https://github.com/user-attachments/assets/eee734f7-9378-4fb3-863c-faab1b04fd5a" />
</div>
