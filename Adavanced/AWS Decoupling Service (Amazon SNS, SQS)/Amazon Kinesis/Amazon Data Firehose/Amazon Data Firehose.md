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

4. demo-heatmap-backend
   - IAM - 사용자 - admin - 액세스 키 발급 - 기타 - 생성
   - 설정 방법
```
1. AWS CLI 설치 및 credential configure (aws configure --profile {프로필명})
2. node 18 이상 설치
3. yarn 설치 : npm install -g yarn
4. 프로젝트 디렉토리에서 Dependency 가져오기 : yarn
5. 배포 : yarn deploy --aws-profile {프로필 명}
```
```
endpoints:                                              
  GET - https://6rm5orkmwg.execute-api.ap-northeast-2.amazonaws.com/dev/athena
  GET - https://6rm5orkmwg.execute-api.ap-northeast-2.amazonaws.com/dev/template
  GET - https://6rm5orkmwg.execute-api.ap-northeast-2.amazonaws.com/dev/athena/cron/partionload
  GET - https://6rm5orkmwg.execute-api.ap-northeast-2.amazonaws.com/dev/athena/cron/query
```

5. demo-heatmap-frontend
```
설치 방법
1. node 18 이상 설치
2. yarn 설치
3. demo-heatmap-backend 프로비전
4. .env 파일 업데이트(AWS Access Key Pair 만들고 Backend API 주소 가져와서 업데이트)
5. yarn / yarn start
```
```
REACT_APP_FIREHOSE_NAME='demo-heatmap-click-firehose-stream'
REACT_APP_ACCESSKEY_ID='{액세스 키}'
REACT_APP_SECRET_ACCESS_KEY='{비밀 액세스 키}' (실전에서는 전용 계정 만들기)
REACT_APP_RECORD_API_ADDRESS='https://6rm5orkmwg.execute-api.ap-northeast-2.amazonaws.com/dev/athena'
```

6. Click Heatmap 생성 : 클릭하게 되면 S3 로그에 저장
   - 개발자 도구 Console 확인
```
Clicked coordinates: X=100, Y=173
MainPage.js:62 {$metadata: {…}, Encrypted: false, FailedPutCount: 0, RequestResponses: Array(1)}
MainPage.js:89 Clicked coordinates: X=114, Y=180
MainPage.js:62 {$metadata: {…}, Encrypted: false, FailedPutCount: 0, RequestResponses: Array(1)}
MainPage.js:89 Clicked coordinates: X=135, Y=173
MainPage.js:62 {$metadata: {…}, Encrypted: false, FailedPutCount: 0, RequestResponses: Array(1)}
...
```

   - Amazon Data Firehose - demo-heatmap-click-firehose-stream - 구성 - Amazon S3 대상 - S3 버킷 접두사 : ```year=!{timestamp:YYYY}/month=!{timestamp:MM}/day=!{timestamp:dd}/```
   - S3 버킷을 통해 확인 : 클릭 한 번 당 하나의 파일이 아닌 Firehose에서 버퍼로 만들어서 저장하다가 일정 기간이 지났거나 용량이 차면 하나의 파일로 생성
```
{"type":"click","page":"main","width":856,"height":323,"x":171,"y":145,"timestamp":1770488406990,"datetime":"2026-02-08 03:20:06"}
{"type":"click","page":"main","width":856,"height":323,"x":171,"y":145,"timestamp":1770488407144,"datetime":"2026-02-08 03:20:07"}
{"type":"click","page":"main","width":856,"height":323,"x":171,"y":145,"timestamp":1770488407304,"datetime":"2026-02-08 03:20:07"}
{"type":"click","page":"main","width":856,"height":323,"x":171,"y":145,"timestamp":1770488407450,"datetime":"2026-02-08 03:20:07"}
{"type":"click","page":"main","width":856,"height":323,"x":179,"y":172,"timestamp":1770488407748,"datetime":"2026-02-08 03:20:07"}
{"type":"click","page":"main","width":856,"height":323,"x":179,"y":172,"timestamp":1770488407881,"datetime":"2026-02-08 03:20:07"}
{"type":"click","page":"main","width":856,"height":323,"x":179,"y":172,"timestamp":1770488408026,"datetime":"2026-02-08 03:20:08"}
{"type":"click","page":"main","width":856,"height":323,"x":182,"y":172,"timestamp":1770488408176,"datetime":"2026-02-08 03:20:08"}
{"type":"click","page":"main","width":856,"height":323,"x":188,"y":174,"timestamp":1770488408305,"datetime":"2026-02-08 03:20:08"}
{"type":"click","page":"main","width":856,"height":323,"x":237,"y":212,"timestamp":1770488408448,"datetime":"2026-02-08 03:20:08"}
{"type":"click","page":"main","width":856,"height":323,"x":266,"y":226,"timestamp":1770488408595,"datetime":"2026-02-08 03:20:08"}
{"type":"click","page":"main","width":856,"height":323,"x":268,"y":226,"timestamp":1770488408746,"datetime":"2026-02-08 03:20:08"}
{"type":"click","page":"main","width":856,"height":323,"x":282,"y":234,"timestamp":1770488408900,"datetime":"2026-02-08 03:20:08"}
{"type":"click","page":"main","width":856,"height":323,"x":299,"y":194,"timestamp":1770488409102,"datetime":"2026-02-08 03:20:09"}
{"type":"click","page":"main","width":856,"height":323,"x":297,"y":191,"timestamp":1770488409270,"datetime":"2026-02-08 03:20:09"}
{"type":"click","page":"main","width":856,"height":323,"x":294,"y":187,"timestamp":1770488409429,"datetime":"2026-02-08 03:20:09"}
```
   - 버퍼 힌트 : 버퍼 크기 (50MiB) 또는 버퍼 간격 (60초) 해당되면 생성

7. 활용
   - AWS Glue - Crawlers - demo-heatmap-click-crawler Run crawler
   - AWS Databases - Tables - 생성된 Log - Schema
   - 쿼리 실행 : test_config.yml (demo-heatmap-backend)
```yml
aws_profile: {프로필명}
app: backend
region: ap-northeast-2
useAWSSDKV3: true
env:
  account_id: {account_id}
  logbucket_name: demo-heatmap-clicklog
  service: demo-heatmap
  resultbucket_name: demo-heatmap-athena-result
  stage: dev
claimsProfiles:

test_targets:

  - uri: athena/cron/partionload.js
    eventType: http
    description: partition load
    method: get
    parms:
      page: main
    expect:
      checkType: check_200

  - uri: athena/cron/query.js
    eventType: http
    description: query
    method: get
    parms:
      page: main
    expect:
      checkType: check_200
```
   - Debug : Debug Jest Tests
   - Click Heatmap 웹 화면의 오른쪽 heatmap 클릭 후 확인
   - 다른 곳 클릭 후, 60초 이후 다시 Debug Jest Tests

8. 리소스 정리
   - 액세스 키 삭제
   - CloudFormation - demo-heatmap-dev-1-serverless 삭제
