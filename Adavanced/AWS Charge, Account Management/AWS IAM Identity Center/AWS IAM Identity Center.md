-----
### 계정 생성
-----
1. 수 많은 IAM 자격증명을 관리해야 함 (IAM user name + password + MFA) X 계정 숫자)
2. 다른 사람에게 권한을 주고 싶을 경우, 계정 숫자만큼 유저를 생성해야 함
3. 모든 계정에 일일히 세팅 필요

-----
### 역할 변경
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/a2944023-154c-49c2-8e60-15dd8109842a" />
</div>

-----
### 역할 전환
-----
1. 하나의 IAM 유저 생성으로 다양한 계정 접근 가능
2. 모든 계정에 일일히 세팅 필요
3. 권한 관리가 어려움
4. 역할 변경을 위한 URL 관리의 어려움
<div align="center">
<img src="https://github.com/user-attachments/assets/cfb0d6e1-ae49-48b5-b45c-55db0fecb80e" />
</div>

-----
### AWS IAM Identity Center
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/65c949c8-88b9-4b60-bc37-0240c929147d" />
<img src="https://github.com/user-attachments/assets/2d344e83-8fbe-4f82-bf4d-b580c4dd85c4" />
<img src="https://github.com/user-attachments/assets/766cebf0-8195-49a5-a23c-ea7f464ee059" />
</div>

1. 인력의 ID를 안전하게 생성하거나 연결하고 AWS 계정 및 애플리케이션 전체에서 이들의 액세스 권한을 중앙에서 관리하는데 도움
2. 모든 규모 및 유형의 조직에서 AWS 내 인력의 인증 및 권한 부여에 사용할 수 있는 권장 접근 방식
3. AWS의 다양한 계정(Organizations 소속)에 단일 로그인을 지원하는 서비스
4. 다양한 자격 증명(Identity) 소스 사용 가능
   - SAML 2.0 지원 소스 (Microsoft Active Directory, On-Premise 기반 등)
   - AWS SSO 자체 자격증명 소스 (무료)

5. IAM와 비슷하게 그룹 기반으로 유저 권한 관리 가능 : 사용자 MFA 설정 가능
6. 로그인 웹 UI를 통해 로그인 : 커스텀 주소 만들기 가능

-----
### 구성 요소
-----
1. 자격 소스 : 자격증명을 검증해줄 수 있는 소스
2. AWS 권한 세트 (Permission Sets)
   - 부여할 권한 집합 (= IAM 역할)
   - 사용자 / 그룹과 연동
   - IAM 정책 같은 형식으로 정의 (JSON)
   - 해당 권한 세트를 AWS 계정에 부여 가능

3. 사용자 : 로그인이 가능한 사용자 (MFA 설정 가능)
4. 그룹 : 사용자의 집합
5. 애플리케이션 : 자격 증명 연동을 지원하는 애플리케이션 등
<div align="center">
<img src="https://github.com/user-attachments/assets/8d4abb89-cd2e-4fc9-ab12-34623c3e7e5a" />
</div>

-----
### 주의사항
-----
1. MFA 사용 설정 가능 : 기본 필수이며 선택에 따라 비활성화 가능 (비추천)
2. MSP 별 Identity Center를 허용하지 않는 경우가 있으므로 주의 필요
3. 별도로 프로그램 액세스 방식 자격증명 발급 가능
   - 즉, AWS SDK / CLI 등에서 활용할 수 있는 자격증명 역시 발급받아 사용 가능
   - 임시자격증명만 발급 가능 : 영구자격증명은 발급 불가
4. 로그인 세션 유효기간 : 최대 12시간

-----
### 구성 순서
-----
1. AWS Identity Center 활성화 (Organizations 관리 어카운트만 가능)
   - Organization 구성 (Root 예하 dev, production 존재 / dev 예하 aws-lecture-qa, aws-lecture-sandbox, production 예하 aws-lecture-production)
   - IAM Identitiy Center 활성화 - 리전 확인 후 활성화
      
2. Custom URL 설정
   - 로그인을 위한 AWS Access Portal URL 편집 : 인스턴스 이름 편집 (aws-lecture) 후, awslecture (이름은 마음대로)

3. 사용자 생성 (이메일 확인 필요)
4. 연동할 그룹 / 사용자 생성
   - IAM Identitiy Center - 그룹 - 그룹 생성 - admin / billing 그룹 생성
   - 사용자 - 사용자 추가 - spark 및 이메일 주소 입력 - 그룹에 사용자 추가 (admin / billing)

5. 부여할 권한 세트 생성
   - IAM Identitiy Center - 권한 세트 - 권한 세트 생성 - admin - 사전 정의된 권한 세트 - AdministratorAccess - 세션 기간 : 12시간
   - Billing 권한 - 사전 정의된 권한 세트 - Billing - 세션 기간 : 12시간

6. AWS 계정과 연결
   - IAM Identitiy Center - AWS 계정 - dev 예하 aws-lecture-qa, aws-lecture-sandbox, production 예하 aws-lecture-production 모두 선택 후 사용자 또는 그룹 할당
    - admin 그룹 - 다음 - AdministratorAccess 권한 세트 - 제출 (4개의 계정에 추가)
    - billing 그룹 - 다음 - Billing - 제출
    - 이메일 확인 : Accept Invitation 
7. 사용자 로그인
    - 암호는 사용하는 암호 입력 후, 로그인
    - MFA 디바이스 등록 (Admin 권한 경우) 후 입력
      + 사용하고 싶지 않다면, 설정 - 인증 - 멀티 팩터 인증 구성에서 설정 가능
    - 로그인하면, 각 계정에 대한 세트 확인 가능
    - 각 권한으로 클릭하면 해당 권한을 가진 계정으로 접속 가능

8. aws-lecture-sandbox에 개발자 권한 부여
   - IAM Identitiy Center - 그룹 - team-dev
   - 사용자 - spark-dev / 이메일 주소 입력 / 그룹 : team-dev 선택
   - 권한 세트 - 사용자 지정 권한 세트 - AmazonEC2FullAccess / 권한세트 이름 : Dev-EC2FullAccess
   - AWS 계정 - aws-lecture-sandbox 계정 - 사용자 또는 그룹 할당 -  team-dev - 권한 - Dev-EC2FullAccess - 제출
   - 이메일 확인 후 수락 후 유저 생성 후 로그인
   - 액세스 키를 누르면, 사용할 수 있는 액세스 키, 시크릿 액세스 키, 세션 토큰 활용가능 (콘솔 로그인 뿐만 아니라 프로그래밍 방식으로도 가능)
   
