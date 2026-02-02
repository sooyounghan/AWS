-----
### Amazon CloudWatch 로그
-----
1. 💡 AWS 서비스 및 온프레미스 서비스에서 수집한 로그를 저장하고 관리하고 확인할 수 있는 서비스 : AWS 대부분 서비스와 기본적으로 연동 (예) AWS Lambda 등)
2. 로그 관리 및 활용을 위한 다양한 기능 제공
   - 로그 모으기 : 다양한 계정에서 하나의 계정으로 모아 관리
   - 실시간 모니터링
   - 로그 수명 주기 관리 : 일정 기간 후 로그를 아카이브 하거나 삭제
   - 로그 쿼리 기능 : 수집한 로그에서 일정 규칙에 따라 로그 쿼리

3. 구성 요소
   - 로그 그룹 : 로그의 관리 단위로 주로 같은 애플리케이션 / 서비스 / 리소스 단위로 분류
     + 예) application-{스테이지}, Lambda 함수명 등
     + 다양한 설정의 기본 단위

   - 로그 스트림 : 같은 소스에서 순차적으로 받는 로그들의 집합 (예) EC2 클러스터로 구성된 웹 서버에서 특정 인스턴스 아이디에서 온 로그 집합)
   - 로그 이벤트 : 타임스탬프와 데이터로 구성한 특정 이벤트의 기록
   - 보존 기간 : 로그가 자동으로 삭제되기까지 걸리는 시간 (무한정 보관 가능)
<div align="center">
<img src="https://github.com/user-attachments/assets/af83f45b-1218-4a36-a19a-a988742d4b59" />
</div>

   - 로그 클래스
     + Standard : 실시간 모니터링이 필요하거나 자주 사용되는 로그 (기본)
     + Infrequent Access : 비용 효율적으로 자주 사용되지 않거나 CloudWatch 일부 기능만 활용 (예) Subscription Filter, Metric Filter, Insight 서비스 등 사용 불가능)
     + 로그 그룹 생성 후 변경 불가능

4. Log Insights
   - 대화식 검색으로 CloudWatch 로그를 분석할 수 있는 서비스
     + JSON 형식으로 구성된 로그를 쿼리 가능
     + 하나의 요청은 최대 20개의 로그 그룹을 동시에 쿼리 가능

   - S3 → OpenSearch 등 별도의 로그 분석 작업 필요 없이 로그 분석 가능
   - 쿼리 저장 및 대시보드로 시각화 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/c3cd90b7-4608-493e-8c00-ac45cb2509c8" />
</div>

5. Metric Filter
   - CloudWatch Log에 필터를 걸어 필터링 된 로그를 지표화시켜주는 기능
   - 특정 필터 패턴과 일치하는 로그만 지표화
      + 예) ```{ $,.eventType="*" && $.sourceIPAddresss!=123.123.* }```
      + 다양한 비교 및 정규식 적용 가능

   - 💡 필터가 적용된 시점부터 지표화

6. 기타 기능
   - Live Tailing : 실시간으로 올라오는 로그를 확인하는 방법 (콘솔 / CLI 활용 가능)
   - 이상 탐지 : 머신러닝을 활용해 로그 그룹 단위로 패턴에서 벗어난 이상 로그 탐지
   - Log Subscription : 실시간으로 다른 서비스 / 보관 장소로 로그를 전달하는 기능
     + 로그에 대한 분석, 로그 전용 계정으로 이관, 백업 등 용도로 활용
     + 필터링 가능
     + 예) S3에 백업, 다른 계정으로 전송, ElasticSearch 등 전달하여 분석

-----
### Demo
-----
1. 로컬 애플리케이션으로 CloudWatch 로그 생성 : 로그 그룹 / 로그 스트림 구조 확인
    - CloudWatch 권한을 가진 IAM 생성 : 사용자 생성 - demo-my-cluodwatch-user / CloudWatchFullAccessV2 - 이후, 보안 자격 증명에서 액세스 키 만들기 - 기타
    - generateLog.js에 액세스 키와 비밀 액세스 키 입력
    - 프로젝트 루트 디렉토리 명령어 실행
```
npm install @aws-sdk/client-cloudwatch-logs
```
   - 디버깅 모드로 코드 실행
   - CloudWatch 로그 그룹 확인 (로그 - Log Management) : my-log-group
     + 테일링 시작
     
2. CloudWatch Insight로 다양한 쿼리 및 시각화 확인
   - 로그 - Logs Insights - 로그 그룹 선택 (my-log-group)
   - insight_queries.txt의 쿼리 실행 : 시각화 가능


