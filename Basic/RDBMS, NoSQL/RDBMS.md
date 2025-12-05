-----
### RDBMS (Relational Database Management System) 
-----
1. 데이터의 관계에 집중하여 정형화된 형식으로 데이터를 관리하는 DB 관리 시스템 : 행과 열로 구성된 2차원 테이블을 기반으로 정형화된 스키마 형식으로 데이터를 저장
2. ACID 형태로 Transaction 처리
3. ACID
   - Transaction : 데이터베이스 상태 변화를 수행하는 작업들을 묶은 단위
     + DB의 상태 변화가 올바르게 이루어질 수 있도록 논리적으로 묶은 단위
     + 예) 타 계좌로 송금할 경우 (본인 계좌에서 돈 차감 + 상대 계좌로 돈 입금)

   - Atomicity (원자성) : All or Nothing, 즉, 전부 성공하거나 실패해야 함
   - Consistency (일관성) : 트랜잭션 수행 전과 후에 일관적으로 데이터가 올바른 상태를 유지헤야 함 (예) 재고량이 음수가 되지 않을 것)
   - Isolation (독립성) : 트랜잭션 간에는 서로 독립적으로 수행되어야 함
   - Durability (지속성) : 트랜잭션 수행 결과는 영구적으로 데이터베이스에 반영되어야 함

4. 데이터 간 관계를 기반으로 데이터 처리 가능 : SQL, Join 등을 활용한 복잡한 쿼리 처리 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/813efc76-eabc-4032-b64e-4e1f21cdfe01" />
</div>

5. 주요 AWS 서비스 : Amazon RDS, Amazon Aurora, Amazon Redshift
6. 주요 사용 사례 : 대부분의 애플리케이션의 메인 DB
