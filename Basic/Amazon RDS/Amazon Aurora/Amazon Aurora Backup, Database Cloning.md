-----
### Aurora의 백업
-----
1. 읽기 복제본(Read Replica) 지원 (Aurora Replica와 다른 개념)
   - MySQL DB의 Binary Log 복제 (Binlog)
   - 단, 다른 리전에만 생성 가능

2. RDS와 마찬가지로 자동 / 수동 백업 가능
   - 자동 백업 : 1 ~ 35일 동안 보관 (S3에 보관)
   - 수동 백업(스냅샷) 가능
   - 백업 데이터를 복원할 경우 다른 데이터베이스를 생성

-----
### Aurora 데이터베이스 클로닝
-----
1. 기존 데이터베이스에서 새로운 데이터베이스를 복제 : 스냅샷을 통해 새로운 데이터베이스를 생성하는 것보다 빠르고 저렴
2. Copy-On-Write 프로토콜 사용 : 기존 클러스터를 삭제해도 제대로 동작
<div align="center">
<img src="https://github.com/user-attachments/assets/a297405b-e0c0-48c5-bac2-023ad0b01f15" />
</div>
