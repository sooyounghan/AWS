-----
### Amazon Athena
-----
1. 표준 SQL를 사용해 Amazon S3에 저장된 데이터를 간편하게 분석할 수 있는 대화식 쿼리 서비스
2. Serverless 서비스로 관리할 인프라가 없으며, 실행한 쿼리에 대해서만 비용 지불
3. 즉, Interactive Query를 통해 S3의 데이터를 조회할 수 있는 서비스 : S3를 SQL 기반 언어로 조회
4. 별도의 프로비저닝 혹은 서버 없이 동작(Serverless)
5. 사용 사례
   - 여러 로그 파일이 저장된 S3에서 필요한 데이터를 조회
   - 정형화된 메타데이터 혹은 저장 데이터 조회
   - 이벤트 데이터에서 필요한 정보를 추출(A / B 테스트) 등
6. 주로 AWS Glue 서비스로 데이터 스키마 생성

-----
### AWS Glue
-----
1. 완전 관리형 추출, 변환 및 로드(ETL : Extract, Transform, and Load) 서비스
2. 효율적인 비용으로 간단하게 여러 데이터 스토어 및 데이터 스트림 간에 원하는 데이터 분류, 정리, 보강, 이동

-----
### 로그 수집 / 분석 아키텍쳐
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/45d5a851-2a86-4d71-a6f2-3c8828497c3f" />
</div>

-----
### Demo - Athena를 활용한 로그 분석
-----
1. S3 로그 버킷 생성
   - 데이터 버킷 : demo-data-bucket-{계정ID}
   - 로그 버킷 : demo-athena-result-{계정ID}

2. 로그 업로드
   - 데이터 버킷에 log_code.txt 업로드

3. AWS Glue Crawler (데이터 분석 서비스)를 사용해 S3 버킷에 저장된 로그 스키마 생성
   - AWS Glue - Data Catalog - Crawlers - Create crawler - demo-my-crawler - Add a data source : S3, Browse S3 선택 후, 데이터 버킷 선택 후, 마지막 / 입력
   - IAM 생성 : AWSGlueServiceRole-demo-my-crawler
   - Add Database : demo-my-database 생성 후 선택
   - Run crawler
   - Data Catalog - Tables를 확인하면, 생성된 테아블 확인 가능

4. Amazon Athena 설정
   - Athena - Athena 콘솔에서 데이터 쿼리 선택 후, 쿼리 편집기 시작
   - 설정 - 관리 - Browse S3 - demo-athena-result-{계정ID} 선택 후 저정
   - 편집기 - 테이블 데이터 버킷의 옆 점 세개 클릭 쿼리 테이블 미리 보기 

5. S3 쿼리 입력 후 실행 (복잡한 조인 불가, 조회만 가능)
   - 쿼리 내용은 demo-athena-result-{계정ID}에 저장

6. 리소스 정리 : 버킷 정리 
