-----
### SQS FIFO (Fist-In First-Out)
-----
1. 기본적으로 SQS는 순서를 보장하지 않음
2. 기본적으로 SQS는 메세지를 여러 번 전달할 가능성 존재
3. FIFO 모드 : 메세지의 순서를 보장하며 단 한 번만 전달
4. 이 외 몇 가지 기능 추가 가능하지만, 성능 저하 존재
   - 예) 300 트랜잭션 per Second (FIFO) VS 거의 제한 없는 트랜잭션 (Standard)
   - High Throughput 모드로 어느 정도 완화 가능
     + 리전 별 조금씩 최대 상한치가 다름
     + 예) us-east : 70,000 / ap-northeast-1 : 9,000 / ap-northeast-2 : 2,400

5. 이름에 .fifo 필수 (예) my-demo-quque.fifo)
<div align="center">
<img src="https://github.com/user-attachments/assets/47192fb4-0c18-46d1-81ab-53e879bb4d8c" />
<img src="https://github.com/user-attachments/assets/6329eaf2-1152-41f4-97a3-f2d0ff1c5e1c" />
</div>

-----
### SQS FIFO - Deduplication ID
-----
1. DeduplicationID : SQS에서 각 메세지의 중복 여부를 판단하기 위한 고유 토큰
2. 메세지를 SQS에 전달할 때 부여할 수 있으며, 한번 특정 Deduplication ID를 가진 메세지가 처리된 경우 다음 5분 동안 같은 Deduplication ID를 가진 메세지는 무시
   - 성공은 정상적으로 반환, 메세지만 무시
   - 메세지가 전달된 이후에도 계속 레코드를 추적
3. 두 가지 방법으로 제공
   - Contents Based : SQS에서 자동적으로 메세지의 Body의 SHA-256 해시를 Deduplication ID로 사용 (💡 즉, Attribute는 HASH 과정에 포함하지 않음)
   - 메세지의 프로듀서가 직접 Duplication ID를 생성해서 같이 전달 (예) Timestamp 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/9e0cf8dd-37bf-4aa9-8f6e-37d173d28516" />
</div>
 
