-----
### AMI (Amazon Machine Image)
-----
1. EC2 인스턴스를 실행하기 위해 필요한 정보를 모은 템플릿
2. 구성
   - 1개 이상의 EBS 스냅샷
   - 사용 권한(어떤 AWS Account가 사용할 수 있는지의 사용 권한)
   - 블록 디바이스 맵핑(EC2 인스턴스를 위한 볼륨 정보 = EBS가 무슨 용량으로 몇 개 붙는지)
3. 필요에 따라 Private으로 가지고 있거나 Public 공개 가능
4. 인스턴스 저장 유형에 따른 AMI 생성 방법
   - EBS : 스냅샷 기반으로 루트 디바이스 생성
   - 인스턴스 저장 : S3에 저장된 템플릿을 기반으로 생성
<div align="center">
<img src="https://github.com/user-attachments/assets/3da69844-6242-40d6-98d3-1d56cab42bb7" />
<img src="https://github.com/user-attachments/assets/86667e38-125e-4bf1-9ec8-f09027a20684" />
<img src="https://github.com/user-attachments/assets/c33ee96e-c7bd-4ba0-bf71-aa12dd08a113" />
</div>

-----
### 정리
-----
1. EBS
   - 가상의 하드 드라이브
   - EC2와 별도의 생명 주기를 가지고 있음
   - 용량과 범위를 자유롭게 설정 가능
   - 총 5가지 유형 제공
     + 범용 (General Purpose or GP) : SSD
     + Provisioning 된 IOPS (Provisioned IOPS or io) : SSD
     + Thtroughput 최적화 (Throughput Optimized HDD or st)
     + 콜드 HDD (SC)
     + 마그네틱 (Standard)

2. 스냅샷
   - 특정 시간에 EBS 상태를 저장한 데이터
   - 증분식

3. AMI
   - EC2 인스턴스를 실행하기 위해 필요한 정보를 모은 템플릿
   - 인스턴스 유형에 따라 다른 생성 방법
     + EBS : 스냅샷을 기반으로 Root 디바이스 생성
     + 인스턴스 저장 : S3에 저장된 템플릿을 기반으로 생성

  
