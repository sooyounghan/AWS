-----
### AWS 계정과 프리티어
-----
1. AWS 계정 (Account)
   - AWS의 사용자와 리소스를 관리하는 단위
     + AWS 계정에 AWS 리소스를 생성
     + 이후 다양한 사용 주체(IAM 사용자, 역할 등)이 생성된 리소스를 사용
     + 💡 계정과 사용자는 다름

   - 계정 생성 방법
     + 이메일, 신용카드, 계정 이름을 제공해서 신규 계정 생성
     + AWS Organization으로 생성

   - 계정 생성 시 Root 사용자를 자동으로 생성

2. Root 사용자
   - 계정을 생성할 때 같이 생성되는 사용자
   - 계정의 모든 권한을 가지고 있으며, 계정 권한을 제한할 방법이 없음
     + 탈취당했을 때 복구가 매우 어려움 = MFA 필수
     + 첫 로그인 이후 35일 안에 MFA 등록 필요

   - Root 사용자만 가능한 작업
     + AWS 계정 설정 변경 (메인 이메일 주소, 계정 이름, 연락처 정보 등)
     + 요금 관련 설정(IAM 유저에게 위임 가능)
     + AWS Support Plan(지원 플랜), 구독, 변경, 취소
     + AWS 계정 삭제

3. IAM 사용자
   - IAM (Identity and Access Management)를 통해 생성한 사용자
   - 생성 시 권한이 따로 부여되어 있지 않으며, 정책 혹은 그룹을 통해 권한을 부여받아 활동
<div align="center">
<img src="https://github.com/user-attachments/assets/98ccb82b-8b77-4b0c-b906-87b23c6792ab" />
</div>

4. AWS 계정의 의미
   - AWS 리소스 관리의 일반적 최대 단위
     + 일반적으로 설정의 가장 큰 단위는 계정
     + 예) 계정 별 최대 S3 최대 버킷 개수 등

   - 관리 / 피해 범위를 한정할 수 있음
     + 권한 관리를 쉽게 할 수 있음
     + 해킹 등 피해 발생 시 피해 범위 최소화 가능

   - 비용 범위에 대한 확인을 쉽게 할 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/502e7ddb-2251-4298-b50b-8f76abb18508" />
<img src="https://github.com/user-attachments/assets/77a1baa6-3108-4840-a5a4-52dce4301aae" />
<img  src="https://github.com/user-attachments/assets/c5d81fad-e5fc-46eb-b0b5-c683a373e13a" />
</div>

5. AWS 다중 계정 아키텍쳐
<div align="center">
<img src="https://github.com/user-attachments/assets/0a9f14b8-73c9-4f2f-9dd3-c58d63b67179" />
</div>

6. AWS 계정 생성
   - 처음 생성할 때 본인 명의 신용카드 필요
   - AWS 계정을 처음 생성하면, Root 사용자와 기본 리소스(기본 VPC) 등 생성
   - AWS 계정 아이디가 부여 (숫자)
     + 로그인할 때 해당 숫자 기억
     + 추후 AWS 계정에 별면 지정 가능 (문자)
   - Root 사용자
     + 생성한 계정의 모든 권한으 자동으로 가지고 있음
     + 생성 시 만든 이메일 주소로 로그인
     + 탈취당했을 때 복구가 매우 힘듬 : 사용을 자제하고 MFA 설정 필요
     + Root 사용자는 관리용으로만 이용 권장 : 계정 설정 변경, 빌림 등

   - IAM 사용자
     + IAM(Identity and Access Management)을 통해 생성한 사용자
     + 만들 때 주어진 아이디로 로그인
     + 기본 권한 없음 : 따로 권한을 부여해야 함
       * 예) 관리자 IAM User, 개발자 IAM User, 디자이너 IAM User, 회계팀 IAM User
       * 권한 부여 시 루트 사용자와 같이 모든 권한을 가질 수 잇지만, Billing 권한은 루트 사용자가 허용해야 함
     + 꼭 사람이 아닌 애플리케이션 등의 가상 주체를 대표할 수 있음
     + AWS 관리를 제외한 모든 작업은 관리용 IAM User를 만들어 사용 권장

7. AWS Free-Tire
   - 고객에게 서비스별로 지정된 한도 내에서 무료로 AWS 서비스를 사용해 볼 수 있는 기능 (누구나 사용 가능 : 학생 ~ Fortune 500위 회사 모두)
   - 2025년 7월 15일 이후 생성된 AWS 계정은 새로운 프리티어 시스템 적용 (즉, 기존 생성 계정은 기존 프리티어 시스템)
   - 신규 계정 생성시 $100 Credit 지급 및 활동에 따라 $100 Credit 추가 지급 (최대 12개월까지 사용 가능 / 추가 활동 예시 : Amazon EC2 Provision, AWS Budget 설정 등)
   - Free Plan 계정 / Paid Plan 계정 분리
     + Free Plan에서 일부 서비스만 사용 가능
     + Free Plan의 경우 6개월, 혹은 모든 크레딧이 소진되면 계정 잠금
     + 비용은 절대 발생하지 않음

8. Free Plan과 Paid Plan
   - Free Plan : AWS를 처음 사용하며 PoC를 적용하거나 실험을 위한 계정 플랜
     + 비용 발생 없음
     + 6개월 혹은 Credit이 만료될 때까지 사용 가능
       * 이후 90일 여유 기간 이후 모든 리소스 삭제 (데이터 다운로드 불가능하므로, Paid Plan으로 필요)
     + 사용 가능한 서비스 제한
       * 거의 왠만한 유명한 서비스들은 모두 사용 가능(EC2, Lambda, SQS, S3, Route53, CloudFront 등)
       * 사용 불가능한 서비스 에시 : Organizations(초대받기는 가능), MarketPlace, Neptune
     + 기타 제한
       * Reserved Instance 등 구매 불가능
       * 제한된 기간 무료 서비스(예) QuickSight) 사용 불가능
       * 프로모션 Credit 사용 불가능
       * Organization에 가입하거나 스킬빌더 Subscription 등의 경우 자동 Paid Plan 승격

   - Paid Plan : 실제 서비스를 운영하기 위한 플랜
     + 매월 발생한 비용을 등록한 신용카드로 청구
     + 즉, 가지고 있는 Credit이 소진 또는 만료될 경우 그 시점부터 비용 청구
     + 기타 모든 기능 사용 가능 (예) RI 구매, 프로모션 크레딧 사용 등)
     + 제한된 기간 무료 서비스 사용 가능(예) QuickSight)
<div align="center">
<img src="https://github.com/user-attachments/assets/b716b181-c888-44ed-8266-12061f6802fc" />
</div>

9. 주의 사항
    - Free Plan은 단 한번만 생성 가능 : 이전 사용 전화번호, Credit Card, 이메일 등 정보로 계정 생성 시 Free Plan 사용 불가능
<div align="center">
<img src="https://github.com/user-attachments/assets/a8d86a90-8846-40b1-b427-dfce9d8fd676" />
</div>

10. Credit 획득 활동
    - 계정 생성시 부여되는 $100 이외에 다양한 활동으로 추가 $100까지 획득 가능
    - 주요 활동
      + Amazon EC2 Provision
      + Amazon BedRock PlayGround 사용
      + AWS Budget 설정
      + AWS Lambda Function URL로 Web-App 생성
      + Amazon RDS DB Provision

   - 주의 사항 : 활동 완료 시점과 관련 없이 획득 크레딧은 계정 생성 후 12개월 이후 만료

11. 언제나 무료 서비스
    - 기존 및 신규 고객 모두에게 일정 시간 동안 (보통 월) 일정 사용량을 무료로 제공하는 방식 : 일정 시간 동안 일정 사용량 이상 사용하면 그 이후부터 과금
    - 예시
      + AWS Lambda (AWS Serverless 컴퓨팅 서비스) : 월 100만건 무료
      + Amazon CloudFornt (AWS의 CDN 서비스) : 월 1TB 데이터 송신, HTTP / HTTPS 천만건, CloudFront 함수 200만건 무료
      + Amazon DynamoDB (AWS의 NoSQL DB 서비스) : 월 25GB 저장 공간, 월별 2억개 처리 용량 무료

12. 제한된 기간 무료 서비스
    - 사용을 시작한 후 일정 기간 혹은 일정 사용량 사용 이후 부터 요금 청구 방식 : 즉, 무료 사용기간을 초과하거나, 무료 사용량을 초과하면 요금 발생
    - 예시
      + Amazon QuickSight (AWS BI / 시각화 툴) : 처음 30일 무료
      + Amazon LightSail (AWS의 컴퓨팅 파워를 쉽게 빠르게 Provision) : 처음 3개월, 월 750시간 무료
      + Amazon Chime (AWS의 커뮤니케이션 툴(Google Meet)) : 30일 동안 PRO 티어 무료

13. 기타
    - AWS에서 인터넷으로 데이터 전달은 월 100 GB 무료 (EC2, S3, ELB 등)
    - 인터넷에서 AWS로 들어오는 트래픽은 언제나 무료

14. 무료 티어 트랙 / 알람
    - AWS Free Tier Usage Agent : 각 서비스별로 85% 정도 도달하면 이메일로 알림
    - 혹은 Free Plan 기간 만료가 가까워지거나 Credit이 거의 소진되었을 때 알람 : 이외에 AWS Budget의 Zero Spend Budget Template으로 좀 더 정확한 알람 가능
    - 트랙 가능한 서비스 : ```https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/tracking-free-tier-usage.html#free-tier-services```
    - Billing and Cost Management에서 프리티어 현재 사용량에 대해 확인 가능

15. AWS의 비용 관리 및 보안
    - Paid Plan에서부터 다양한 이유로 원치 않는 요금 발생 가능 (예) EC2의 Provision 후 정리하지 않은 경우 등)
    - 이 외 해킹 및 보안 사고 때문에 비용 발생 가능 (예) EC2 / SageMaker 등 탈취해 비트코인 채굴)

16. AWS 계정 보안 설정
    - 루트 사용자 및 IMA 사용자의 MFA
    - 비용 모니터링 설정 : AWS Budget
    - AWS의 리소스 모니터링
      + AWS Config
      + AWS User Notification
