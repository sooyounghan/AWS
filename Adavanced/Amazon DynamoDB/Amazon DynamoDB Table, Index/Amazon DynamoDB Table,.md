-----
### DynamoDB 테이블
-----
1. 테이블 : 다양한 아이템의 집합
2. DynamoDB의 다양한 작업 단위 (백업 / 내보내기 / 권한 등)
3. 하나의 테이블에는 반드시 하나의 파티션 키(Partition Key 또는 Hash Key)를 위한 속성 지정 필요 (선택적으로 정렬 키(Sort Key 또는 Range Key)를 위한 속성 지정)
4. 두 가지 클래스
   - Standard : 표준
   - Standard-Infrequent Access : 저장은 저렴하지만 처리(읽기, 쓰기) 비용은 더 높음
5. Capacity Mode
   - On-Demand : 읽고 쓴 용량만큼 과금
  
   - Provisioned Capcity : 시간 단위 당 읽고 쓸 수 있는 용량(WCU, RCU)을 미리 지정해서 과금
     + On-Demand에 비해 5배이상 저렴
     + 필요시 Reserved Capacity를 활용하여 추가 할인 가능
     + 초당 허용된 RCU / WCU 초과시 Throttling 발생
     + AutoScaling 가능

6. AutoScaling
   - Capacity를 상황에 따라 동적으로 조절해주는 기능
   - Target Utilization, 즉, 수렴하고자 하는 용량을 정해서 알고리즘에 따라 Capacity 조절
     + 1분 단위로 측정하여 2번 이상 연속적으로 특정 사용량을 넘어선다면, Scale Up = 2분 이상 필요
     + 1분 단위로 측정하여 15번 이상 연속적으로 특정 사용량보다 적다면, Scale Down = 15분 이상 필요
  - 내부적으로 CloudWatch를 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/aec5e7ef-1124-470d-bc83-a6858476ab39" />
</div>

7. 테이블 구조 : 각 DynamoDB 테이블은 PartitionKey의 Hash 값을 기준으로 각 10GB 단위 분산 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/5e705bda-33d1-42d5-8ca4-fb04c293521d" />
</div>

   - 각 파티션 단위로 3000 RCU / 1000 WCU를 넘을 경우 Throttling 발생
   - 즉, 파티션 키는 최대한 Unique하게 유지 필요
