-----
### Amazon CloudWatch 지표
-----
1. 지표(Metric) 수집
   - 시간 순서로 정리된 데이터 집합 : 다수의 데이터 포인트로 구성
   - AWS 대부분 서비스는 기본적으로 지표 제공 (예) EC2 CPU 사용량, 네트워크, Disk IO 등)
   - 커스텀 지표 생성 가능 : 유저가 직접 원하는 데이터포인트를 생성해 CloudWatch로 전달하여 생성 (예) EC2의 메모리 사용량 등)
   - Region 단위
   - 15개월 이후 사라짐 : 지속적으로 새로운 데이터가 들어올 경우 15개월 이전 데이터는 사라지는 형식

2. 네임스페이스 / 지표 이름
   - 네임스페이스
     + CloudWatch 지표의 컨테이너
     + 지표의 출신 혹은 성격에 따라 논리적으로 묶은 단위
     + AWS에서 수집하는 기본적인 지표는 AWS/{서비스명} 형식 (예) AWS/EC2, AWS/RDS)
     + 💡 반드시 직접 명시해야 함 (필수) : 디폴트 없음
<div align="center">
<img src="https://github.com/user-attachments/assets/d4f99d2c-f397-46bd-8f2a-d673524ba251" />
</div>

   - 지표 이름 (필수) : 지표의 고유 이름 (무엇에 관한 지표인지?) 

3. 데이터 포인트(Data Points)
   - 지표를 구성하는 시간-값 데이터 단위 (시간은 초 단위까지 / 예) 2016-10-31T23:59:59Z)
   - UTC 기준이 매우 권장됨 : 내부적인 통계 혹은 알람 등에서 UTC 기준으로 활용
   - Resolution : 데이터가 얼마나 자주 수집되는지 나타내는 개념
     + 기본적으로 60초 단위 수집
     + High Resoultion 모드에서 1초 단위
     + 이후 초 단위로 1, 5, 10, 30 혹은 60의 배수로 조회 가능

   - Period : 데이터가 얼마만큼의 시간을 기준으로 묶여서 보여지는지에 관한 개념
     + 초 단위로 1, 5, 10, 30 혹은 60의 배수
     + 60초 단위 미만은 High-Resoultion 모드의 데이터만 가능
     + 최소 1초에서 최대 86,400초 (1일) 까지 가능
     + 보관 기간 : 60초 미만은 최대 3시간 / 60초는 15일 / 300초는 63일 / 1시간(3600초)는 455일(15개월)
     + 작은 단위의 보관 기간은 큰 단위로 계산 합쳐짐 (예) 1분 단위는 15일 동안 확인 가능, 이후 15일이 지나면 5분 단위로만 확인 가능, 이후 63일이 지나면 1시간 단위로만 확인 가능)
     + 💡 주의 사항 : 2주 이상 데이터가 업데이트 되지 않은 Metric의 경우 콘솔에서 보이지 않음 (모든 콘솔에서 사라지며, CLI로만 확인 가능)

4. 차원(Dimension)
   - 일종의 태그 / 카테고리
     + Key-Value로 구성
     + Metric(지표) 구분에 사용
   - 최대 30개까지 할당 가능
   - 예) AWS/EC2 네임스페이스에 모든 EC2의 지표가 수집됨 - 이후, InstanceID Dimension를 통해 EC2 인스턴스 단위로 구분해서 확인
   - 조합 가능 (예) Dimension: {Server=prod, Domain=Seoul} or {Server=dev}

5. 기타 : Unit - 단위(%, Byte, Seconds, Count 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/198d9cdf-a059-42f0-ab88-baa0520336ad" />
<img src="https://github.com/user-attachments/assets/df05cb24-fce4-44f4-8977-cce26c92f24b" />
</div>

6. 기타 기능
   - 지표 시각화 : 지표들을 확인하고 적절한 그래프로 시각화 가능 (다양한 지표를 선택해 비교 분석 가능)
   - Metric Insight : 지표를 SQL 형식으로 조회하고 통합하여 분석할 수 있는 기능
     + 예) SELECT AVG(CPUUtilization) FROM SCHEMA("AWS/EC2", InstanceId)
     + 자연어로 쿼리 생성 가능 (현재 US-East / US-West / AP-Northeast-1 Only)
     + 예) EC2 인스턴스 중 가장 높은 Network Out을 기록하고 있는 인스턴스 출력
    
