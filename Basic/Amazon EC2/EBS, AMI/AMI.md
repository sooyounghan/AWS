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

4. Amazon EC2의 백업
   - 인스턴스 스토어 EC2
   - EBS 기반 EC2
    
5. AMI
   - EC2 인스턴스를 실행하기 위해 필요한 정보를 모은 템플릿
   - 구성
     + 1개 이상의 EBS 스냅샷
     + 사용 권한(어떤 AWS Account가 사용할 수 있는지의 사용 권한)
     + 블록 디바이스 맵핑(EC2 인스턴스를 위한 볼륨 정보 = EBS가 무슨 용량으로 몇 개 붙는지)
<div align="center">
<img src="https://github.com/user-attachments/assets/c33ee96e-c7bd-4ba0-bf71-aa12dd08a113" />
</div>

-----
### Snapshot 생성
-----
1. 수동 생성
2. 자동 생성
   - AWS Backup
   - Amazon Data Lifecycle Manager (AWS Snapshot Lifecycle Policy) : EBS 스냅샷 및 EBS-backed AMI의 생성, 보존 및 삭제를 자동화할 수 있음
     + EBS 스냅샷 및 AMI 생성, 보관, 삭제, 관리를 자동화하는 서비스
     + 다양한 활용 예
       * 주기적 백업 및 AMI 갱신
       * 감사 및 관리 목적 백업 생성
       * 오래된 백업 자동 삭제를 통한 비용 최적화
     + 다양한 AWS 서비스와 연동 가능
       * CloudTail, CloudWatch Event 등
       * 예) 스냅샷 생성 여부 / 완료 여부 등을 Event로 받아 Slack 처리 가능
     + 무료
     + 구성 요소
       * 스냅샷 / EBS 기반 AMI
       * 타겟 리소스 태그 : DLM에서 백업 대상을 구분하기 위해 검사하는 대상의 태그
         * 예) stage=prod, backup-required=yes, environment=live
         * 태그 목록 중 하나라도 존재 시 대상으로 판정 (OR)
       * Data Lifecycle Manager 태그 : 생성된 스냅샷 / AMI에 적용하는 고유 태그
         * aws:dlm:lifecycle-policy-id
         * aws:dlm:lifecycle-shcedule-name
         * aws:dlm:expirationTime
         * dlm:managed
         * (optional) aws:dlm:archived
       * Lifecycle Policy : 실제 백업 방법을 정의한 정책
         * 정책 타입
           * 스냅샷 정책
           * EBS 기반 AMI 정책
           * Cross-Account Copy Event 정책 : 다른 게정으로 스냅샷을 복사하고 싶을 때 사용
         * 리소스 타입
           * Volume : EBS 볼륨 대상
           * Instance : EC2 인스턴스 대상
             * 해당 EC2 인스턴스에 붙은 모든 볼륨에 대해서 백업
           * 타겟 태그
           * Schedule : 백업이 언제 수행되고 얼마나 보관되는지 정의
   - Amazon EventBridge Cron Event + Lambda

-----
### 정책 스케줄
-----
1. EBS 스냅샷 혹은 AMI가 언제 생성되는지 정의
2. 각 정책은 4개의 Schedul 보유 가능
   - 한 개는 필수, 3개는 선택
   - 예) 일 단위, 월 단위, 연 단위 생성 스케줄 정의 가능
3. 설정 내용 : 빈도 / 태그 / 교차 계정 방식 / 복구 방식 등

-----
### AWS Backup
-----
1. 정책을 기반으로 대규모 데이터를 간편하고 비용 효율적으로 보호할 수 있는 안전관리형 서비스
2. On-Premise 및 AWS 서비스 전반에서 백업을 자동화 하는 서비스
3. 주요 기능
   - 자동화 백업 관리
   - 백업 보존 관리
   - 백업 모니터링
   - 암호화
   - 규정 준수 보고
4. EC2, EBS, EFS, DynamoDB, RDS, Aurora, VMWare 등 사용 가능
5. 다른 Region, 다른 Account에 백업 가능
6. EC2 스냅샷, AMI 뿐만 아니라 보안그룹, VPC 설정, 역할 등 까지 저장 가능
7. Backup Valut에 저장
   - Backup Valut : 백업의 저장 장소 단위
   - AWS Backup 사용 가능한 Region에 추가로 생성 가능
8. 백업 뿐만 아니라 복원 과정까지 지원 : 복원 시 해당 보안 그룹, VPC 설정, 역할 등 모두 해당 시점으로 복원
9. Data Lifecycle Manager보다 최신 서비스 : 더 많은 AWS 서비스와 기능 지원
