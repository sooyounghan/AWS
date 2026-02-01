-----
### Amazon Aurora DB Cluster
-----
1. 하나 이상의 DB 인스턴스와 데이터를 관리하는 Cluster 볼륨을 묶은 단위
   - Cluster 볼륨 : 데이터베이스의 저장 공간으로 여러 가용 영역에 걸쳐 데이터를 복제 분산 저장
2. 구성
   - Primary (Write) DB 인스턴스 : Read / Write 모두 가능한 인스턴스 (클러스터 당 하나)
   - Aurora Replica (Reader DB 인스턴스) : Cluster 볼륨에 접근 가능한 DB 인스턴스로 Read만 지원 (클러스터 당 최대 15개 보유 가능)
3. 인스턴스 간 Async 복제
4. Write가 죽을 경우 자동으로 Replica 중 하나가 Writer로 Fail-Over
   - 데이터 손실 없이 Fail-Over 시 메인으로 승격 가능
   - 고가용성(High Availability) 확보
<div align="center">
<img src="https://github.com/user-attachments/assets/19956a42-92a6-416a-8db6-03b9dcd2b6c6" />
</div>

