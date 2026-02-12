-----
### Amazon CloudFront Monitoring
-----
1. CloudFront는 다양한 방법으로 모니터링 가능
2. Amazon CloudFront CloudWatch 지표 (Metric)
   - CloudWatch를 통해서 다양한 지표(Metric) 제공
   - 기본 지표와 추가 비용으로 활성화 가능한 지표
     + 기본 지표 : 요청 숫자 / Byte Downloaded / Byte Uploaded / 4xx, 5xx, Total Error Rate
     + 추가 지표 : Cache Hit Rate / Origin Latency / Error Rate by Status Code (401, 403, 404, 502, 503, 504)

   - 추가 지표 비용은 고정이며, 활성화 지표 별로 한달에 한 번 발생 (Per Distribution)
   - 버지니아 리전(US-East-1)
<div align="center">
<img src="https://github.com/user-attachments/assets/b09a2b43-cc0e-411a-bdae-4710f63a6c7c" />
</div>

3. Amazon CloudFront Access Log 
   - Amazon CloudFront Access Log (Standard Log) : S3 버킷을 지정해서 모든 유저의 요청을 로깅 (실시간이 아니며, 지연시간 발생)
<div align="center">
<img src="https://github.com/user-attachments/assets/0a2f9a63-ecb7-404c-80f2-756d38615f3c" />
</div>

   - Real Time Log : 약 초단위의 지연시간으로 요청을 실시간으로 로깅
   - S3 버킷을 지정해서 모든 유저의 요청을 로깅
   - Distribution 단위로 요청받은 Edge Location에서 지속적으로 Log 파일을 만들어 S3 버킷으로 Flush
   - 시간 단위로 여러 번 Flush
     + 최대 24시간 지연 가능
     + 💡 심지어 아예 전송되지 않고 누락될 수 있음
     + 헤더와 쿠키의 크기가 20KB가 넘거나 URL이 8192 Bytes가 넘어갈 경우 CloudFront에서 요청을 별도로 Parse하지 않고 처리 (즉, 이 경우에는 로깅이 되지 않음, Body는 문제 없음)

3. Amazon CloudFront Access Log Real Time Log
   - CloudFront의 요청 로그를 실시간으로 처리할 수 있는 기능
     + Kinesis Data Stream으로 처리 : 추후 Firehose 등으로 S3에 로그로 저장, RedShift / OpenSearch 등으로 분석 가능
   - 추가 비용 발생
   - 로그의 지연시간이 발생하거나 누락될 수 있음
   - 설정 값
     + Sampling Rate : 요청 중 얼마만큼을(퍼센트) 받아볼 것인지에 관한 설정 (1 ~ 100)
     + Fields : 로그 내용 중 실시간으로 받아볼 필드 (Timestamp, Client IP, Time to First Byte, Status Code, Host, Edge Location, Time Taken 등)
     + Cache 동작 : 실시간 로그를 받아볼 패턴 단위의 동작 (Behavior)
   - Kinesis Stream 권한 설정에 IAM 역할 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/00f0f9ad-0fa5-4a9a-bef3-42fa94c22b6d" />
</div>

4. CloudFront Log Format / 목적지 제어
   - CloudFront에 S3 이외에 추가로 Cloudwatch와 Firehose로 로그를 전달할 수 있는 기능
   - JSON과 Apache Parquet 로그 포맷 선택 가능
     + 추후 Athena 등으로 분석 가능
     + 신규로 S3로 로그 저장 시 Delimiter (\n) 추가 기능 : 기존엔 Lambda 등으로 처리 필요
   - CloudWatch로 전달 시 요청당 750 Byte는 무료 (S3 로그 전송의 경우 항상 무료)

5. Amazon CloudFront 보고서 및 분석
   - AWS에서 제공하는 CloudFront의 통계 및 분석 페이지 (CloudFront - 보고서 및 분석 - 캐시 통계. 인기 객체, 상위 레퍼러, 사용량, 뷰어 등)
   - 캐시 통계, 인기 객체, 레퍼러, 사용량, 뷰어 종류 등을 배포 별로 확인 가능

6. Demo - CloudFront 로깅 확인
   - CloudFront + S3 Origin
     + S3 버킷 두 개 생성 : demo-cf-origin-bucket-{계정 ID} / demo-cf-log-bucket-{계정 ID}
     + CloudFront - 배포 생성 - demo-loging-cf - Amazon S3 : demo-cf-origin-bucket-{계정 ID} / 보안 보호 비활성화
     + demo-cf-origin-bucket-{계정 ID} S3 버킷에 index.html 업로드
     + Standary Log Destinations
       * CloudFront demo-loging-cf - Logging - Add - Amazon S3 - 대상 S3 버킷 : demo-cf-log-bucket-{계정 ID}
       * 분할 : {DistributionID}/{yyyy}/{MM}/{dd}/{HH}
       * Output Format : JSON
       * 필드 선택 : Distribution, timestamp 포함
     
   - 요청을 보내고 S3 로깅 설정 : Athena + Glue로 분석
     + 배포된 demo-loging-cf의 Domain Name/index.html을 복사해 웹 브라우저에 전송 : index.html 내용 출력 / 잘못된 경로 접속 후 로그 확인
     + JSON 형식으로 로깅
     + Glue - Crawlers - demo-my-cf-log-crawler - Data Source Configuration : Not Yet / Data Source : Browse S3 - demo-cf-log-bucket-{계정 ID}/AWSLogs/ 까지 선택
     + IAM : IAM 생성 - AWSGlueServiceRole-demo-cf-log
     + Add Database : demo-my-cf
     + Crawler 생성 및 awslogs와 관련 테이블 생성

   - Athena - 쿼리 편집기 탐색 - 설정 - 관리 - demo-athena-result-{계정 ID} 생성 후 설정
     + 데이터 원본 : AwsDataCatalog
     + 데이터베이스 : demo my-cf
     + 테이블 - 테이블 미리보기

   - 리소스 삭제 : S3 Bucket 삭제
