-----
### Amazon EBS
-----
1. Amazon Elastic Block Store(EBS) : AWS 클라우드의 Amazon EC2 인스턴스에 사용할 영구 블록 스토리지 볼륨 제공
   - 각 Amazon EBS 볼륨은 가용 영역 내 자동으로 복제되어 구성요소 장애로부터 보호해주고, 고강용성 및 내구성 제공
   - 즉, 가상 하드 드라이브로, EC2 인스턴스가 종료되어도 계속 유지 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/6c11b577-49a0-4441-a634-ebe8b8d8dd08" />
<img src="https://github.com/user-attachments/assets/07f2e3d4-ada3-4ef5-96f7-5549ae604481" />
</div>

2. EBS(Elastic Block Stroe)
   - 가상 하드 드라이브
   - EC2 인스턴스가 종료되어도 계속 유지 가능
     + Root 볼륨으로 사용 시 EC2가 종료되면 같이 삭제
     + 단, 설정을 통해 EBS만 따로 존속 가능
   - 용량을 범위에 따라 자유롭게 설정 가능
   - 특수하게 하나의 EBS를 여러 EC2에 장착 가능한 경우도 존재 (EBS Multi Attach)
   - 💡 EC2 인스턴스와 같은 가용영역에 존재 : 즉, 하나의 가용영역 안에서만 존재
   - 가용영역 안에 자동으로 분산 저장 : 99.99% 가용성 목표

3. EBS 유형
   - 범용 (General Purpose or GP) : SSD
   - Provisioning 된 IOPS (Provisioned IPOS or io) : SSD
   - Throughput 최적화 (Throughput Optimized HDD or st) : HDD
   - 콜드 HDD (SC) : HDD
   - 마그네틱 (Standard) : HDD
<div align="center">
<img src="https://github.com/user-attachments/assets/0f9d9e33-2d22-4bf8-8a5d-14884579d93c" />
</div>

4. Snapshot
   - EBS의 특정 시점을 저장한 이미지
     + 이후 EBS로 다시 복구 가능
     + EBS 백업 용도로 활용 가능
   - 증분식 : 바뀐 부분만 저장
     + 즉, 100GB 볼륨의 스냅샷을 5번찍어도 500GB가 아닌 100GB + 4번의 변경 부분만 저장
     + 비용 역시 쵲거화
   - S3에 저장 : 99.99% 내구성
   - Data Lifecycle Manager / AWS Backup 등으로 자동화 해서 생성 가능
   - 기타 여러 기능
     + 암호화
     + 아카이브 : 더 적은 비용으로 저장할 수 있으나 몇 가지 제약사항 적용
       * 최소 90일 이상 저장
       * 복원에 최대 72시간 소요
     + 공유 : 다른 계정 등에 공유 가능
     + EBS Direct API를 활용해 스냅샷에 직접 내용을 쓰거나 읽기 가능
