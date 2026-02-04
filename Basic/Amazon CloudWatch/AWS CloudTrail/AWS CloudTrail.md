-----
### AWS CloudTrail
-----
1. AWS 계정의 거버넌스, 규정 준수, 운영 감사, 위험 감사를 지원하는 서비스
2. AWS 인프라에서 계정 활동과 관련된 작업을 기록하고 지속적으로 모니터링하며 보관할 수 있음
3. AWS Management Console, AWS SDK, 명령줄 도구 및 기타 AWS 서비스를 통해 수행된 작업을 비롯해 AWS 계정 활동의 이벤트 기록을 제공
4. AWS의 보안 및 감시를 위한 서비스 : 감시(CCTV)
5. AWS의 여러 서비스에 대한 활동 이벤트 로그 등 제공
   - 이벤트 예시 : EC2 종료, S3 버킷 생성, IAM 사용자 삭제 등
   - 이벤트의 시간 및 결과 / 에러, 사용 인증 정보 등을 기록
   - AWS CLI, 콘솔 이용, API 호출 등 모든 이벤트가 대상

6. 기본적으로 90일 이벤트 로그를 무료로 저장 : 이 이상 기록은 Trail 생성 필요
7. 실시간 기록이 아니며, 지연시간 발생 (약 5분)

-----
### AWS CloudTrail Trail
-----
1. S3 기반 이벤트 로그 수집 단위 : 즉, S3 버킷 생성 후 지정 필요
2. Region 기반 (단, 다중 리전 혹은 단일 리전 선택 가능)
   - 다중 리전일 경우 모든 리전의 로그를 한 곳으로 모아 관리 가능
   - 신규 리전이 추가될 경우, 자동으로 로깅
   - 💡 글로벌 서비스의 경우 : us-east-1 리전에서 로깅 (IAM, STS, CloudFront)
     + 즉, 다중 리전 로깅 혹은 us-east-1에 Trail 생성 필요
   - 별도로 AWS의 다양한 서비스(EventBridge, CloudWatch)로 이벤트 전달 설정 가능
     + Metric Filter / 이벤트 트리거 가능
     + 💡 5분 이상 지연 시간 가능성 염두 필요

-----
### AWS CloudTrail Event
-----
1. AWS 계정의 활동 내용 : 몇몇 Data API의 경우 수동으로 활성화 필요(S3, Lambda, Dynamo DB 등)
2. JSON 형식으로 데이터 저장 : Athena 등으로 분석 가능
3. 총 3가지 종류
   - Management Event : AWS 계정 관리를 위한 이벤트
     + 예) IAM 역할 부여, VPC 생성 / 삭제, 서브넷 생성 / 삭제, Trail 생성, 로그인 이벤트 등
     + 기본 로깅 대상

   - Data Event : 리소스의 동작과 관련된 데이터
     + 예) S3의 Object 동작 (Get, Delete, Put), Lambda 함수 호출, SNS 호출 등
     + 💡 별도로 로깅 활성화 필요 + 추가 비용 발생

   - Insight Event : CloudTrail에서 별도로 생성하는 정상적이지 않은 상황에 대한 감지 이벤트
     + 예) 평소와 다른 S3 버킷 100개 삭제, Deny된 EC2 인스턴스 프로비전 요청이 분당 수십 개 발생
     + 💡 별도로 로깅 활성화 필요 + 추가 비용 발생
<div align="center">
<img src="https://github.com/user-attachments/assets/8c216322-02bb-441f-b64d-94d0fada41d0" />
<img src="https://github.com/user-attachments/assets/a460262c-6909-4479-9d2e-7ec19500dc62" />
</div>

-----
### Demo - CloudTrail
-----
1. CloudTrail 생성 및 모니터링 확인
    - CloudTrail - 추적 생성 - Demo-My-Trail / 새 S3 버킷 생성 / AWS KMS (Key Management System) : Demo-Cloud-Trail-KMS / CloudWatch Logs - 활성화 됨 / IAM 역할 생성 필요 - CloudTrailRoleForCloudWatch-My-Demo-Trail
    - 관리 이벤트, 데이터 이벤트 선택 - 리소스 유형 : S3 / 모든 이벤트 로그
      
2. CloudTrail 생성 : Data Event 수집 활성화
   - EC2 : demo-my-ec2 / 보안 그룹 default

3. CloudShell에서 EC2 정보 조회(Describe-Instance) API 호출 및 로깅 확인
   - CloudTrail에서 이벤트 기록 - RunInstances 확인
   - 인스턴스 종료 후 확인 : 속성 조회 - 이벤트 소스 / ec2.amazonaws.com - TerminateInstances

4. S3 Object 생성 및 요청 이벤트 확인
   - S3 이벤트 확인 : S3 버킷에 파일 업로드 : s3.amazonaws.com
     + 기본 트레일에서 보이지 않음 : 따라서, 트레일 기반이 되는 Log Bucket으로 진입하여 AWSLogs/ - 362454057659/ - CloudTrail/ - ap-northeast-2/ - 해당 날짜 - 로그 확인하면, 이벤트 로그 기록 확인 가능
     + CloudTrail - 이벤트 기록 - Amazon Athena에서 테이블 생성 - S3 버킷 생성 - 테이블 생성
     + Athena 콘솔에서 데이터 쿼리 - 쿼리 편집기 시작
     + 쿼리 설정 - 관리 : 저장할 S3 버킷 지정 (aws-athena-query-results-{계정 ID}-ap-northeast-2
   - CloudTrail에서 생성된 기본 테이블에서 다음 SQL 실행
```sql
SELECT * FROM "default"."{테이블 이름}" where eventsource='s3.amazonaws.com' and eventname='PutObject' and useragent !='cloudtrail.amazonaws.com' order by eventtime desc
```

5. S3 버킷 정리 및 추적 삭제

-----
### CloudTrail vs CloudWatch
-----
1. CloudTrail
   - AWS를 감사(Audit)하기 위한 서비스 : 감시(CCTV)
   - AWS의 모든 서비스가 사용될 때마다 사용 로그 저장
   - AWS가 언제, 어디서, 누구에 의해 사용되었는가?
   - 단순하게 AWS 사용 로그만 저장

2. CloudWatch
   - AWS를 모니터링 하기 위한 서비스 : Performance 확인
   - AWS 서비스 뿐만 아니라 애플리케이션의 로그 및 동작 로그 취합
   - 애플리케이션이 어떻게 동작 되었는가? 무슨 버그가 있었는가? 메모리는 얼마나 소모되었는가?
   - 대시보드 및 알람 등 모니터링을 위한 서비스 제공
   
