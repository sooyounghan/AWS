-----
### Amazon RDS (Relationship Database Service)
-----
1. 클라우드에서 관계형 데이터베이스를 간편하게 설정, 운영 및 확장할 수 있음
2. 하드웨어 프로비저닝, 데이터베이스 설정, 패치 및 백업과 같은 시간 소모적인 관리 작업을 자동화하면서 비용이 효율적이고, 크기 조정이 가능한 용량을 제공
3. 사용자가 애플리케이션에 집중하여 애플리케이션에 필요한 빠른 성능, 고가용성, 보안 및 호환성을 제공할 수 있도록 지원
4. 즉, 관계형 데이터베이스를 제공하는 서비스 (Relationship Database Service : 관계형 데이터베이스 ↔ NoSQL (Dynamo DB, Documnet DB, Elastic Cache))
5. 가상 머신 위에서 동작 : 단, 직접 시스템에 직접 로그인 불가능 (OS 패치, 관리 등은 AWS의 역할)
6. RDS는 Serverless 서비스가 아님 (단, Aurora Serverless는 말 그대로 Serverless 서비스)
7. 암호화 지원 및 자동 백업 지원

-----
### RDS와 EC2
-----
1. 내부에서는 EC2를 활용
2. VPC 안에서 동작
   - 기본적으로 Public IP를 부여하지 않으면, 외부에서 접근 불가능
   - 설정에 따라 Public IP를 부여하여 인터넷에서 접근 가능 (DNS로 접근)
3. 서브넷과 보안그룹 지정 필요
4. EC2 타입 지정 필요
5. 스토리지는 EBS 활용 : EBS 타입 및 용량 선택 필요
6. 💡 중지 가능 : 단, 7일 후 다시 시작됨
7. 이 외 백업으로 스냅샷 생성 가능
8. 별도로 Reserved Instance 활용 가능 : 일정 기간 약정하여 최대 70% 정도 할인

-----
### RDS 아키텍쳐
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/ffc19778-a821-488a-af4f-36ab1330774d" />
</div>

-----
### RDS의 인증방법
-----
1. 전통적인 유저 / 패스워드 방식 : AWS Secrets Manager와 연동하여 자동 로테이션 가능
2. IAM DB 인증 : 데이터베이스를 IAM 유저 자격증명 / 역할을 통해 관리 가능
3. Kerberos 인증

-----
### RDS에서 제공하는 DB 엔진
-----
1. MS SQL Server, Oracle(Oracle OLAP) : 오픈 소스가 아니므로 라이선스 비용 추가
2. MySQL Server
3. PostgreSQL
4. MariaDB
5. IBM Db2
6. Amazon Aurora

-----
### RDS Multi AZ
-----
1. 두 개 이상 AZ에 걸쳐 데이터베이스를 구축하고, 원본과 다른 DB(Standby)를 자동으로 동기화(Sync)
2. 원본 DB의 장애 발생 시 자동으로 다른 DB가 원본으로 승격됨 (DNS가 Standby DB로 동기화)
3. StandBy DB는 접근 불가
4. 퍼포먼스 상승 효과가 아닌 안정성을 위한 서비스
<div align="center">
<img src="https://github.com/user-attachments/assets/e8e09f77-74b6-453d-9690-d533ba72e1ee" />
</div>

-----
### 읽기 전용 복제본
-----
1. 데이터베이스의 읽기 전용 복제본을 생성 (Async)
2. 쓰기는 원본 데이터베이스에, 읽기는 복제본에서 처리하여 워크로드 분산
3. 안정성이 아닌 퍼포먼스를 위한 서비스
4. 원본 DB 장애 발생 시 수동으로 DNS 변경이 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/2173ac72-8d1e-488b-b83e-836620838db9" />
</div>

-----
### RDS Multi Region
-----
1. 다른 리전에 지속적으로 동기화시키는 DB 클러스터 생성 : Async 복제
2. 주로, 로컬 퍼포먼스 혹은 DR 시나리오로 활용

-----
### DB Subnet Group
-----
1. RDS가 프로비전되는 서브넷을 묶은 그룹
   - 실전에서는 주로 프라이빗 서브넷만을 사용 : 보안적인 이유
   - 최소 두 개 이상의 같은 리전의 서브넷이 필요

2. 퍼블릭 접근을 허용하기 위해 퍼블릭 서브넷으로 구성 필요

-----
### Parameter Group
-----
1. 데이터베이스의 주요 파라미터 (타임존, 패스워드 유효기간, 기본 CharSet 등)를 묶은 논리적 단위
2. 미리 저장해 둔 파라미터 설정 모음으로 여러 RDS에 적용 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/311f2f22-3030-4635-8d39-58b0580bfaee" />
<img src="https://github.com/user-attachments/assets/5672015a-d58a-44d3-8e66-b91494de6f1d" />
</div>

-----
### Demo - RDS Provision
-----
