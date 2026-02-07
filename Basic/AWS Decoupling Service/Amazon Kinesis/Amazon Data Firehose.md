-----
### Amazon Data Firehose
-----
1. Amazon Simple Storage Service (Amazon S3), Amazon Redshift, Amazon OpenSearch 서비스, Splunk 및 타사 서비스 (Datalog, LogicMonitor Dynatrace, MongoDB, New Relic , Coraloc) 등의 대상에 실시간 스트리밍 데이터를 전송하는 완전 관리형 서비스
2. 데이터를 원하는 주체로 전달해주는 서비스
   - 주로 데이터를 처리하는 주체(OpenSearch, RedShift, Splunk, Datadog)로 전달
   - 혹은 S3에 직접 저장해서 보관 / Athena 분석 가능
3. 일정 버퍼 / 시간이 찰 때까지 기다렸다가 하나의 파일로 Flush 하는 형식 : 전달 대상에 따라 버퍼 사이즈와 시간 변동
<div align="center">
<img src="https://github.com/user-attachments/assets/1075d9a5-d40f-4976-990e-6c1522f8f2fd" />
<img src="https://github.com/user-attachments/assets/45c75141-9b4b-456d-8540-895276f9ed97" />
</div>

-----
### Data Transform
-----
1. 데이터가 Firehose로 내용을 변경시켜 받는 기능
   - 데이터 포맷, 파일 사이즈 등 데이터 내용을 원하는 대로 변경 가능
   - 예) Apache Parquet으로 변경, 로그 중 민감정보 제거, 필터링 등

2. Apache Parquet 또는 Apache ORC 데이터 포맷 변경은 기본 지원
3. 다른 변경(예) CSV, 민감 정보 제거 등)은 AWS Lambda 서비스를 호출해 변경 : 기본 1MB 단위 ~ 최대 6MB(Lambda Payload Limit)로 전달

-----
### 기타
-----
1. S3에 저장 시 오브젝트 Key(파일명) 지정 가능
2. 예) Athena에서 활용하기 위해 포맷
   - ```<prefix>/year=<YYYY>/month=<MM>/day=<DD>/hour=<HH>/minute=<mm>/<unique_id>.json```
   - 예) myapp/logs/year=2024/month=07/day=05/hour=14/minute=23/abc.12345.json

-----
### Demo - Click Heatmap
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/8ab0fec6-2f10-4718-a3a1-d8f18a5b5e76" />
</div>

1. 아키텍쳐
<div align="center">
<img src="https://github.com/user-attachments/assets/3ebbb05f-b317-465f-ba68-a98d4a959ae4" />
</div>

2. 클릭 로그
<div align="center">
<img src="https://github.com/user-attachments/assets/be06eceb-b735-49b6-91f4-8332d5c118b7" />
</div>

3. Amazon Athena
<div align="center">
<img src="https://github.com/user-attachments/assets/a0e177b5-75e4-4c1f-882a-d8161d865e83" />
</div>
