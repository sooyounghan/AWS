-----
### S3 스토리지 클래스
-----
1. 다양한 스토리지 클래스 제공
   - 클래스별로 저장의 목적, 예산에 따라 다른 저장 방법 적용
   - 총 9가지 클래스
<div align="center">
<img src="https://github.com/user-attachments/assets/ce6472a1-56d2-4490-8c4c-0bc5f4ea1b66" />
</div>

2. S3 스탠다드
   - 99.99% 가용성
   - 99.99999999999% 내구성
   - 최소 3개 이상 가용영역에 분산 보관
   - 최소 보관 기간 없음, 최소 보관 용량 없음
   - 요청 비용 : $0.0045 / 1000 Requests (ap-northeast-2 기준)
   - 저장 비용 : $0.025 / GB (ap-northeast-2 기준)

3. S3 스탠다드 IA (Infrequently Accessed)
   - 자주 사용되지 않는 데이터를 저렴한 가격에 보관
   - 최소 3개 이상 가용영역에 분산 보관
   - 최소 저장 용량 : 128 KB
   - 최소 저장 기간 : 30일
   - 데이터 요청 비용 발생 : 데이터를 불러올 때마다 비용을 더 많이 지불(per GB)
     + 요청 비용 : $0.01 / 1000 Requests VS $0.0045 / 1000 Requests (Standard) (ap-northeast-2 기준)
   - 사용 사례 : 자주 사용하지 않는 파일 중 중요한 파일
   - $0.0138 / GB VS $0.025/GB (Standard) (ap-northeast-2 기준)

4. S3 One Zone-IA
   - 자주 사용되지 않고, 중요하지 않은 데이터를 저렴한 가격에 보관
   - 단, 한 개의 가용 영역에만 보관
   - 최소 저장 용량 : 128 KB
   - 최소 저장 기간 : 30일
   - 데이터 요청 비용 발생 : 데이터를 불러올 때마다 비용을 더 많이 지불(per GB)
     + 요청 비용 : $0.01 / 1000 Requests VS $0.0045 / 1000 Requests (Standard) (ap-northeast-2 기준)
   - 사용 사례 : 자주 사용하지 않으며, 쉽게 복구할 수 있는 파일 (예) 오래된 썸네일)
   - $0.011 / GB VS $0.0138/GB (Standard) (ap-northeast-2 기준)

5. S3 Express One Zone
   - 매우 빠른 퍼포먼스를 위해 하나의 가용영역에 위치한 특별한 저장소에 저장
     + MilliSecond 단위의 응답 속도 (약 10배 빠름)
     + Standard와 비교해 50% 저렴한 요청 비용
   - Amazon S3 Directory Bucket에 저장
   - 컴퓨팅 리소스와 스토리지 리소스를 같은 공간에 위치시켜 더 빠른 액세스 가능
   - 일부 리전만 사용 가능 (ap-northeast-2 사용 불가능)
   - 저장 비용 $0.16 / GB (us-east-1 Region 기준)

6. S3 Glacier Instant Retrieval
   - 아카이브용 저장소
   - 최소 저장 용량 : 128 KB
   - 최소 저장 기간 : 90일
   - 바로 액세스 가능
   - 사용 사례 : 의료 이미지 혹은 뉴스 아카이브 등
   - $0.005 / GB (ap-northeast-2 기준) = Standard의 약 1/5

7. S3 Glacier Fiexible Retrieval
   - 아카이브용 저장소
   - 최소 저장 용량 : 128 KB
   - 최소 저장 기간 : 90일
   - 분 ~ 시간 단위 이후 액세스 가능
   - 사용 사례 : 장애 복구용 데이터, 백업 데이터 등
   - $0.0045 / GB (ap-northeast-2 기준)

8. S3 Glacier Deep Archive
   - 아카이브용 저장소
   - 최소 저장 용량 : 40 KB
   - 최소 저장 기간 : 90일
   - 데이터를 가져오는데 12 ~ 48시간 소요
   - 사용 사례 : 오래된 로그 저장, 사용할 일이 거의 없지만 법적으로 보관해야 하는 서류 등
   - $0.002 / GB (ap-northeast-2 기준)

9. S3 Intelligent-Tiering
   - 머신 러닝을 사용해 자동으로 클래스 변경
   - 퍼포먼스 손해 / 오버헤드 없이 요금 최적화

10. S3 on Outputs
    - 온프레미스 환경에 S3 제공
    - 내구성을 확보한 상태로 파일을 저장하도록 설계
    - IAM, S3 SDK 등 사용 가능

11. 정리 : 목적과 비용에 따라 9가지 클래스
    - S3 스탠다드
    - S3 Express One Zone
    - S3 스탠다드 IA
    - S3 One Zone-IAA
    - S3 Glacier Instant Retrieval
    - S3 Glacier Fiexible Retrieval
    - S3 Glacier Deep Archive
    - S3 Intelligent-Tiering
    - S3 on Outputs
