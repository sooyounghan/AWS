-----
### AWS Organizations
-----
1. 다중 계정의 문제점
   - 생성의 어려움 : 신용 카드 확보, 정보 입력, 인증 등
   - 관리의 어려움 : 자격 증명 생성, 보안 관리, 비용 및 결제 관리, 접근 관리 및 제어, 감사 등

2. AWS Organizations
   - AWS 리소스가 늘어나고 확장됨에 따라 환경을 중앙 집중식으로 관리하고 규제하는데 도움이 됨
   - 이를 통해 프로그래밍 방식으로 새 AWS 계정을 생성하고 리소스를 할당하며, 계정을 그룹화하여 워크플로우를 구성하고, 거버넌스를 위해 계정이나 그룹에 정책을 적용하며, 모든 계정에 대해 단일 결제 방법을 사용하여 청구를 간소화할 수 있음
   - 여러 Account를 중앙 집중식으로 관리할 수 있도록 도와주는 서비스
   - 주요 기능
     + AWS 계정 관리 : 계정의 생성 및 관리
     + 조직의 정의 및 관리 : 계정 생성 시 조직 단위(OU)로 그룹화하여 리소스를 묶어 관리하고 권한을 설정 가능
     + 계정 보안 / 액세스 및 권한 제어 : 여러 계정의 액세스를 중앙에서 관리할 수 있으며, 서비스 제어 정책을 적용해 OU 단위로 권한 제어 가능
     + 중앙 집중식 결제 및 비용 관리 : 각 계정에서 발생한 비용을 통합으로 관리하고 결제 가능
   - 기본 총 10개(확장 가능)의 계정을 링크 : 모든 계정의 비용을 하나의 계정에서 지불
<div align="center">
<img src="https://github.com/user-attachments/assets/33b95cb3-eb29-4e4c-a8bf-2f6adc739635" />
</div>

   - 하나의 Organization은 단 하나의 관리 계정과 0개 이상의 멤버 계정으로 구성
     + 관리 계정 : 가장 상위의 계정으로 Organziations의 소유자 게정
     + 멤버 계정 : Organizations에 속한 계정
       * 초대를 통해 가입
       * 혹은 Organizatins에 직접 생성

   - 모든 AWS 게정은 하나의 Organization 소속만 가능
   - 관리 조직(Organization Unit, OU) 단위로 계정 관리 가능 : 다수의 계정을 하나의 단위로 묶은 개념
   - SCP 혹은 다양한 정책을 설정하여 OU에 속한 모든 계정에 권한 관리 정책 적용 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/d9e68f0a-2557-47c9-b7c3-3d25e8261970" />
</div>

3. 모드
   - 모든 기능
     + 권한 설정으로 통합 결제 기능 포함 AWS Organization의 모든 기능을 사용 가능
       * AWS Organizations을 활성화하면 기존의 계정에 초대장을 보내 Organization으로 가입 신청
       * AWS Organizations이 활성화 되면 조직 안에 새로운 계정을 생성 가능 (신용 카드 필요 없음)
     + SCP (Service Control Policy) 등의 정책을 통해 계정들의 권한 관리 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/e2f8c14e-f0d5-4ba8-a802-36e778afc0b6" />
</div>

   - 통합 결제 기능
     + 간단히 각 계정의 비용을 합산해 관리하고 청구하는 기능만 활성화
     + 일반적으로 하나의 Billing Account와 다수의 Account로 구성
<div align="center">
<img src="https://github.com/user-attachments/assets/95f80198-334e-4e2a-9a37-7d5687622f2d" />
<img src="https://github.com/user-attachments/assets/a46b3924-03c4-4fbe-80dc-86f4ae805a53" />
</div>

4. SCP (Service Control Policy)
   - Organizations에 속한 Account의 권한 범위를 설정 가능 : '어디'까지 가능한가 설정 (실제 권한을 주지 않음)
   - OU 단위, 혹은 계정 단위로 설정 가능
   - 적용 받는 계정의 모든 권한 제어 가능
     + Root 유저도 포함
     + 예) SCP에서 EC2를 금지시켰다면, 해당 계정의 Root 유저도 EC2 접근 불가

   - 사용 예
     + Prod 계정에 Live 리소스만을 모아두고 인턴 / 주니어 레벨 접근 금지
     + 회사 계정 전체에 SageMaker 사용 금지
     + 특정 로그 저장 계정에 S3 Delete 권한 금지
<div align="center">
<img src="https://github.com/user-attachments/assets/85628269-a4e6-4bd1-9dc4-61d66fe109ae" />
<img src="https://github.com/user-attachments/assets/2426d19d-a6f9-4927-9c38-035a61687eff" />
<img src="https://github.com/user-attachments/assets/5789a530-1cdf-4c27-aa95-c34bc9a644f0" />
</div>

5. 기타 기능
   - 멤버 계정의 루트 권한 제한 간으 : 멤버 계정의 루트 사용자를 삭제하고 접근 제한
   - SCP 이외에 다양한 권한 제어 : 백업 정책 / 태그 정책 / EC2 선언적 정책 등
   - 계정 생성 시 자동으로 계정 안에 계정 간 Assume Role 전환을 위한 Role 생성
     + 기본 이름 : OrganizationAccountAccessRole
     + 해당 역할로 Assume Role을 통해 신규 계정 로그인 가능
