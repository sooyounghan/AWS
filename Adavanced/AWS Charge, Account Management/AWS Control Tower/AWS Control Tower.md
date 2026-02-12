-----
### AWS Control Tower
-----
1. 규범 모범 사례에 따라 AWS 다중 계정 환경을 설정하고 관리하는 간단한 방법 재공
2. AWS의 다중 계정을 쉽고 빠르게 프로비전해주는 서비스
   - Organization + Identity Center + CloudFormation + 기타 관리 기능
   - 단순 계정 생성 이외에 보안 설정, 인프라 구현 규칙 설정 등 여러 가지 기능
3. 내부적으로 AWS의 다양한 서비스 활용 : CloudFormation, Organization, Config, Identity Center 등
4. 주요 구현 요소
   - Landing Zone : 모범 사례를 기반으로 설계된 다중 계정 환경 (OU(조직 단위), 계정, 사용자 등이 포함된 전사 리소스의 컨테이너)
   - Guard Rails : 전사 리소스의 규칙을 정하고 예방 / 탐지 / 제어하는 리소스
   - Account Factory : 자동화된 환경에서 구성된 템플릿 기반의 계정을 생성하는 리소스
   - 대시보드 : Landing Zone 전체를 모니터링 할 수 있는 대십드

-----
### AWS Control Tower Landing Zone
-----
1. 최초 생성 시 콘솔에서 설정한 리전을 Home Region으로 설정 : 주요 리소스들은 Home Region에 프로비전
2. 기본 생성 OU
   - Foundational OU(Security OU - 기본 이름) : 보안과 감사를 위한 OU
   - Additional OU(Sandbox OU - 기본 이름) : 기본적으로 생성되며 필요에 따라 다양한 OU 생성
     + AWS 추천
       * Infrasturcture OU : 주로 공유 서비스와 네트워크 리소스 (예) Transit Gateway, Route 53 등)
       * Sandbox OU : 개발 전용 Workload OU (실제 워크로드 생성)
     + 기타 Policy OU, Exception OU, Deployment OU 등

   - 내부적으로 Organizations + Identity Center + CloudFormation 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/56cb0a13-8647-407d-8977-50328eabf738" />
</div>

-----
### AWS Control Tower Guard Rails 주요 정책
-----
1. Mandatory Controls : 필수로 지켜야 하는 규칙으로, 해제할 수 없으며 Security OU를 제외하고 모두 적용 (예) 로그 버킷 삭제 금지)
2. Proactive Controls : 리소스 프로비전 전에 준수를 체크하는 규칙 (예) Dynamo DB 생성 시, 반드시 PITR 백업 활성화)
3. Preventive Controls : 특정 행동에 제약을 설정하는 규칙
   - 주로 SCP로 설정하며, 'enforced', 'not enabled' 상태
   - 예) 4xlarge 이상의 EC2 생성 금지
4. Detective Controls : 특정 상태가 준수되고 있는지 지속적으로 체크하며, 준수되지 않은 리소스 리포트
   - 주로 Config로 설정
   - 예) 보안그룹 All Inbound Open 금지

-----
### AWS Account Factory
-----
1. CloudFormation + Organization 기반으로 AWS 계정 프로비전 (Guardrails 리소스 자동으로 설정)
2. Identity Center에 자동으로 사용자 생성 가능

-----
### Demo - Control Tower를 활용한 Landing Zone 설정
-----
1. 주의 : 모두 프로비전에 1시간 이상 소요
2. 삭제 시에도 2시간 이상 소요 가능 (리소스 숫자에 따라)
   - Demo의 경우 30분 이내 완료
   - 참고 : 삭제 후에도 Organizations은 남아있음
