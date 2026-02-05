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
