-----
### Amazon Aurora
-----
1. 상용 데이터베이스 비용의 1/10으로 완전한 MySQL 및 PostgreSQL 호환성을 통해 전 세계적으로 탁월한 고성능 및 가용성을 제공
2. MySQL의 5배, PostgreSQL의 3배 처리량을 제공
3. MySQL 및 PostgreSQL과 호환되는 완전 관리형 관계형 데이터베이스 엔진 : AWS에서 클라우드 환경에 최적화 된 엔진을 자체 개발
4. 용량의 자동 증감 : 10GB부터 시작하여 10GB 단위로 용량 증가 (최대 128 TB)
5. 연산 능력 : 128vCPU와 메모리 1024 GB 까지 증가 가능 (db.r6i.32xlarge)
6. 데이터의 분산 저장 : 각 AZ마다 2개의 데이터 복제본 저장 X 최소 3개 이상의 AZ = 최소 6개의 복제본
   - 3개 이상을 잃어버리기 전 쓰기 능력 유지
   - 4개 이상을 잃어버리기 전에는 읽기 능력 유지
   - 손실된 복제본을 자가 치유 : 지속적으로 손실된 부분을 검사 후 복구
   - Quorum 모델 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/75f578b5-f17d-45ff-92e7-18e1d8e55c6b" />
<img src="https://github.com/user-attachments/assets/14c26084-3128-47f0-8800-b57d91c7a720" />
</div>
