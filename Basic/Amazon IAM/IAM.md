-----
### IAM
-----
1. AWS Identity and Access Management(IAM)로 AWS 서비스와 리소스에 대한 액세스를 안전하게 관리하며, AWS 사용자 및 그룹을 만들고 관리하며 AWS 리소스에 대한 액세스를 허용 및 거부할 수 있음
2. 즉, AWS의 보안 및 관리를 담당하는 글로벌 서비스
3. 주요 기능
   - AWS Account 관리 및 리소스 / 사용자 / 서비스 권한 제어
     + 임시 권한 부여
     + 서비스 사용을 위한 인증 정보 부여
   - 사용자 생성, 관리, 계정 보안
     + Multi-Factor Authenticator
     + 사용자의 패스워드 정책 관리
   - 다른 계정과의 리소스 공유

4. IAM 구성
<div align="center">
<img src="https://github.com/user-attachments/assets/8627f8ca-f2a0-44aa-b3bd-33ac8a632d53" />
</div>

5. Root 사용자
   - 계정을 생성할 때 같이 생성되는 사용자
   - 계정의 모든 권한을 가지고 있으며, 계정 권한을 제한할 방법이 없음 : 탈취당했을 때 복구가 매우 어려우므로 MFA 필수
   - 자격증명 부여 가능
   - Root 사용자만 가능한 작업
     + AWS 계정 설정 변경 (메인 이메일 주소, 계정 이름, 연락처 정보 등)
     + 요금 관련 설정 (IAM 유저에게 위임 가능)
     + AWS Support Plan (지원 플랜) 구독 / 변경 / 취소
     + AWS 계정 삭제

6. IAM 사용자
   - IAM(Identity and Access Management)를 통해 생성한 사용자
   - 생성 시 권한이 따로 부여되어 있지 않으며, 정책 또는 그룹을 통해 권한을 부여받아 활동
   - 자격 증명 부여 가능(Optional)
   - 콘솔 로그인 가능(Optional)
   - 사람 뿐만 아니라 애플리케이션과 같은 다른 주체도 대표 가능 (예) 애플리케이션이 AWS 서비스를 사용하기 위해 IAM 사용자의 자격증명 보관 후 사용)

7. 그룹
   - IAM 사용자의 집합
   - 그룹에 정책을 부여하여 그룹에 속한 모든 사용자에게 해당 정책에 따른 권한 부여 가능

8. 역할 (Role)
   - AWS의 권한의 집합
   - 역할에 맞는 다양한 권한의 집합을 만들고 IAM 사용자, AWS 서비스, 애플리케이션 등 부여받아 행동
<div align="center">
<img src="https://github.com/user-attachments/assets/15b03001-5aba-4fee-8c4e-9005fd44942d" />
<img src="https://github.com/user-attachments/assets/c624523d-9cdb-4dd7-8575-2b61ecfc5444" />
</div>

9. 정책 (Policy)
    - 사용자와 그룹, 역할이 무엇을 할 수 있는지 관한 문서
    - JSON 형식으로 정의
    - 그룹 / 역할 / 유저 등에 부여되어 각 주체가 행동 가능한 권한 정의
    - 구성
      + Resource : 어떤 AWS 리소스에 대해
      + Action : 어떤 행동을
      + Effect : 허용 / 거부
      + Condition : 정책이 적용되는 조건 (예) IP 주소, 시간, 태그)
<div align="center">
<img src="https://github.com/user-attachments/assets/d52b5a23-fdc4-4140-8c16-a6c43a64a066" />
</div>

10. 권한 검증 과정
<div align="center">
<img src="https://github.com/user-attachments/assets/353a1564-c01e-4262-bce3-ee6233d5239d" />
</div>


   - EC2에서 S3 서비스를 이용하고 싶을 경우
<div align="center">
<img src="https://github.com/user-attachments/assets/b364c656-5c51-4e7a-ad71-3dba4fcfe3be" />
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/1023fe94-e083-4711-b769-3d829469cad6" />
</div>
