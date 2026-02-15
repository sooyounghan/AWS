-----
### Memory DB
-----
1. 데이터를 메모리에 저장하는 데이터베이스 (↔ Disk 저장)
   - 읽기 / 쓰기 속도가 매우 빠름
   - 휘발성 : 영속성 확보를 위해 별도 조치 필요
   - 주로 Key-Value 모델 NoSQL

2. 단위 활용 비용이 전통적 RDBS보다 비싸지만, 빠르기에 주로 빠른 속도가 필요한 애플리케이션에 활용 (예) 게임 / 금융 트레이딩 플랫폼 / 라이브 스트리밍 등)
3. 두 가지 용도
   - 메인 DB : 빠른 데이터 처리, MSA 아키텍쳐 등
   - 캐싱 : RDBMS 또는 NoSQL 메인 DB의 부하를 경감시키기 위함

4. Amazon Memory 기반 DB : Amazon MemoryDB, Amazon ElasticCache
   
