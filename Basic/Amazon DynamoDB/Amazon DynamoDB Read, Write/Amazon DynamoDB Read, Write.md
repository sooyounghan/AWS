-----
### Amazon DynamoDB 읽기 / 쓰기
-----
1. 강력한 일관성 (Strong Consistency) : 읽기 시점 이전의 모든 작업 결과 반영을 보장하는 일관성
<div align="center">
<img src="https://github.com/user-attachments/assets/6d918b48-dc75-4d3d-af7e-2ff2dba13de7" />
</div>

2. 최종 일관성 (Eventual Consistency) : 읽은 시점 직전의 작업의 결과가 반영되지 않을 수 있는 일관성
<div align="center">
<img src="https://github.com/user-attachments/assets/76156f91-6456-4ae2-bd37-d1a57577ca84" />
<img src="https://github.com/user-attachments/assets/1f9f3b37-2258-4114-b84d-b6b94c611245" />
<img src="https://github.com/user-attachments/assets/ab3a343b-82fb-45a9-9d2b-75642e886006" />
<img src="https://github.com/user-attachments/assets/3d57696b-5275-4237-ba05-7a0156193b2f" />
</div>

3. 트랜잭션 읽기 / 쓰기 (Transactional Read / Write) : 여러 작업 내용을 하나의 작업으로 묶어서 성공 / 실패로 구분할 수 있는 읽기 / 쓰기
4. Read Capacity Unit(RCU) : 초당 4KB(4096 Byte)의 데이터를 읽을 수 있는 티켓
   - 초당 강력한 일관성 읽기 1번(4KB)은 티켓 하나 필요
   - 초당 최종 일관성 읽기 1번(4KB)은 티켓 0.5개 필요 (= 티켓 하나로 최종 일관성 두 번 수행 가능)
   - 초당 트랜잭션 읽기 1번(4KB)은 티켓 두 개 필요
5. Write Capacity Unit(WCU) : 초당 1KB(1024 Byte)의 데이터를 쓸 수 있는 티켓
   - 초당 일반 쓰기(1KB)는 티켓 하나 필요
   - 초당 트랜잭션 쓰기 1번(1KB)은 티켓 두 개 필요
6. 모든 계산은 최소 단위로 올림해서 계산
<div align="center">
<img src="https://github.com/user-attachments/assets/fb5cf95d-d040-4c41-8bc9-d71680e04db7" />
<img src="https://github.com/user-attachments/assets/b68c9c72-a28e-4f2d-a5aa-6b684d49f944" />
<img src="https://github.com/user-attachments/assets/536f88c6-a0c5-4095-a7ef-e5bcb44b1608" />
<img src="https://github.com/user-attachments/assets/9239cedc-3ff1-4b4a-a24c-96a42ea60294" />
</div>

7. Burst Capacity
   - 300초(5분)동안 사용하지 않은 RCU와 WCU를 모아서 Brust에 활용
   - 갑자기 많은 양의 요청이 올 때 혹은 큰 읽기 쓰기 작업에 사용
   - 혹은 내부적인 유지 보수에 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/a690acea-01dd-47d0-bd80-5a063a049d73" />
<img src="https://github.com/user-attachments/assets/bb6fb186-53ea-4534-a4e5-425d09eb127d" />
</div>

8. 읽기 용량
   - 단위 : 4KB (예) 3.5KB : 4KB로 취급, 10KB는 12KB로 취급)
   - 종류
     + GetItem : 항목 하나(파티션 키 / 정렬 키로 구분)를 읽어오는 작업 (4KB로 올림 처리해서 용량 계산)
     + BatchGetItem : 100개까지 항목을 한 번에 읽어오는 작업 (GetItem의 집합 : 각 항목을 4KB로 올림 처리해서 가져올 숫자만큼 곱해서 용량 계산)
     + Query : 특정 파티션 키를 가지고 있는 항목만을 가져오는 작업 (가져오는 모든 항목의 용량을 계산해서 4KB 단위로 올림)
     + Scan : 테이블의 모든 항목을 읽어오는 작업 (테이블 전체의 데이터 용량 계산)

9. 쓰기 용량
   - 단위 : 1KB (예) 0.5B : 1KB로 취급)
   - 종류
     + PutItem : 항목 하나(파티션 키 / 정렬 키로 구분)를 쓰는 작업
       * 새로 생성하는 항목의 용량으로 계산
       * 만약 해당 키가 존재한다면, 기존 항목을 덮어씀 (둘 중 큰 크기로 용량 계산)

     + UpdateItem : 항목 하나를 수정하는 작업
       * 업데이트 전 / 후 항목 중 높은 항목으로 용량 계산
       * 항목의 속성을 하나만 업데이트 하더라도 항목 전체의 용량으로 계산

     + DeleteItem : 특정 항목을 삭제하는 작업 (삭제한 항목의 용량으로 계산)

10. 쓰기
    - 종류
      + BatchWriteItem - 최대 25개의 항목을 쓰는 작업
        * 삭제 / 쓰기 둘 다 가능, 업데이트 불가능
        * 각 쓰기 / 삭제를 1KB로 올림 처리 후, 숫자만큼 곱해서 용량 계산

    - 조건 쓰기 : 특정 조건을 만족할 경우에만 쓰기 허용 (예) Count가 1이라면 2로 업데이트)
      + 업데이트와 로직 동일 (항목 전 / 후를 비교해 높은 용량으로 계산)
      + 티켓 두 배(2 WCU 필요)
    
    
