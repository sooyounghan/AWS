-----
### SNS FIFO(First In First Out)
-----
1. SNS의 메세지 전달을 FIFO로 할 수 있는 모드
2. 두 가지 효과
   - 순서 보장
   - 중복 제거

3. SQS FIFO / Standard Queue만 연동 가능 : 즉, 이메일 / 휴대폰 / SMS / HTTP 엔드포인트 등 일반적인 Subscriber와 연동 불가능
4. 기타 기능
   - 메세지 그룹
   - 메세지 필터링
5. 이름에 .fifo 필수 (예) my-demo-topic.fifo)
<div align="center">
<img src="https://github.com/user-attachments/assets/f8abe9cc-e2be-480e-a470-b318007ac3e5" />
</div>

6. Message 순서
   - 일반적인 SNS + SQS 조합은 순서를 보장하지 않음
   - 즉, 메세지가 발송된 순서와 별도로 대상이 수신
   - 일반 SNS / 일반 SQS 둘 다 순서 보장되지 않음
   - 순서를 보장하기 위해서는 SNS FIFO + SQS FIFO 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/ebf7c8b6-5f3c-444e-bf10-c9e056503962" />
</div>

   - 각 Subscription에 전달되는 메세지 별 순서는 다를 수 있지만, 받는 메세지 순서는 동일
<div align="center">
<img src="https://github.com/user-attachments/assets/b3fec9e9-4431-4b95-bf91-13f0722b75fa" />
</div>

   - Message Sequence Number : 연속적은 아니지만 항상 증가하는 번호
     + SNS FIFO에서 메세지를 받아 발송할 때 부여
     + Message Body에 포함 : 단, Raw Message 전달 활성화 시 포함하지 않음
<div align="center">
<img src="https://github.com/user-attachments/assets/577036d8-f61b-462d-a84b-4ca1d61ccfc9" />
</div>

7. Message Group ID
   - Message Group ID : SNS / SQS FIFO 내부의 일종의 채널
   - Message Group ID 단위로 순서 보장 및 전달이 이루어짐
   - SNS FIFO에서 Message Group ID를 전달했을 때, 대상이 SQS FIFO라면 Message Group ID 같이 전달
<div align="center">
<img src="https://github.com/user-attachments/assets/028fc63d-d9f8-420e-96b6-f562bf148f3b" />
</div>

8. Filtering
   - 메세지 필터링을 통해 모든 메세지 대신 특정 메세지만 수신 가능
   - 각 Subscription Filter Policy 설정 가능
     + Filter Policy : 메세지의 Body 혹은 Attributes 단위로 원하는 메세지 매칭
     + 매칭 시 메세지 발송, 불일칠 시 메세지를 전달하지 않음
<div align="center">
<img src="https://github.com/user-attachments/assets/5b5de071-e564-4eb3-a1ec-f0509068cfe8" />
</div>

9. Deduplication
    - Deduplication : 중복 제거 → 정확하게 한 번만 메세지를 전달하는 기능
    - Deduplication ID를 기반으로 중복된 메세지를 제거
      + 5분 이내 같은 Duplication ID를 가진 메세지가 전달되면, 요청은 성공하지만 전달되지 않음
      + SNS 메세지가 전달된 이후 ID는 트랙

    - SNS에 전달된 메세지 Body를 기반으로 Deduplication ID 생성 가능
      + 메세지 Body의 Hash 기반으로 Deduplication ID를 생성
      + Attributes는 Hash에 반영하지 않음
<div align="center">
<img src="https://github.com/user-attachments/assets/b2660027-33dc-4646-8900-88ace4ec2423" />
</div>

10. Message Archive / Replay
    - SNS FIFO에 전달된 메세지를 저장하고 필요에 따라 Replay 가능
      + 사용 사례
        * 메세지 전달 과정의 오류를 복구
        * 기존 애플리케이션의 장애 복구
        * 신규 애플리케이션의 Sync 맞추기

    - 메세지 보관 기간 설정 : 1일에서 365일
    - 추가 보관 비용 발생
      + 저장 (서울 리전 기준) : $0.025 GB / Month
      + 처리 (서울 리전 기준) : $0.13 / GB

    - Reply 시 시간을 정해서 Replay 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/497c7ea0-1c0e-43ef-93bb-60209a16c01a" />
<img src="https://github.com/user-attachments/assets/024e2519-798d-436f-9afe-624a71d3284c" />
<img src="https://github.com/user-attachments/assets/a1445c9c-8cdd-4bdb-a693-38a04aedade4" />
<img src="https://github.com/user-attachments/assets/0dbba17a-542a-40e0-b3aa-42b49dbc0fc8" />
<img src="https://github.com/user-attachments/assets/23296528-0077-44c3-8bbd-869ea5b6989a" />
</div>
