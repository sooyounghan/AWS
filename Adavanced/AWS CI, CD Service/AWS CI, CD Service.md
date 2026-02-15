-----
### AWS CI / CD 서비스
-----
1. Continuous Integratio / Continuous Delivery (CI / CD)
   - 애플리케이션 개발 단계를 자동화하여 애플리케이션을 보다 짧은 주기로 고객에게 제공하는 방법
   - 자주 빌드하고 자주 배포 : 유저에게 빠르게 제품 전달
     + 더 빠르게 버그가 수정되고 유저의 요구사항이 반영될 수 있는 시스템
     + 더 안정적으로 애플리케이션이 배포될 수 있는 시스템

2. CI / CD Pipeline (AWS Code Service)
<div align="center">
<img src="https://github.com/user-attachments/assets/618cb169-df64-4dd2-817e-450ea9682f34" />
</div>

3. AWS CodeCommit
   - Private Git 레포지토리를 호스팅하는 안전하고 확장성이 뛰어난 관리형 소스 제어 서비스
   - AWS 버전 Github
   - IAM Credential을 통해 접근 가능 (기존 HTTP 프로토콜 / SSH 역시 지원)
   - AWS의 다양한 서비스와 연동
   - 이벤트 버스를 통한 이벤트 기반 로직 처리 가능
   - 서비스 종료(~ 2024. 8.) / 기존 계정에서만 사용 가능하며, 신규 계정에서는 사용 불가능

4. AWS CodeBuild
   - 소스 코드를 컴파일하는 단계에서부터 테스트 실행 후 소프트웨어 패키지를 개발하여 배포하는 단계까지 마칠 수 있는 완전관리형 지속적 통합 서비스
   - CI / CD 중 CI를 담당하는 서비스
   - 별도 프로비전 불필요
   - 간단하게 빌드 세팅이 된 서버 하나를 빌려서 사용하는 개념

5. AWS CodeDeploy
   - Amazon EC2, AWS Fargate, AWS Lambda 및 On-Premise 서버와 같은 다양한 컴퓨팅 서비스에 대한 소프트웨어 배포를 자동화하는 완전관리형 배포 서비스
   - 빌드 / 테스트 제품을 배포해주는 서비스
   - 다양한 배포 모드 (In-Place, Blue / Green) 등 지원
   - 배포 실패 시 자동으로 롤백 지원
   - 배포 상태 모니터링 가능

6. AWS CodePipeline
   - 빠르고 안정적인 애플리케이션 및 인프라 업데이트를 위해 릴리즈 파이프라인을 자동화하는 데 도움이 되는 완전관리형 지속적 전달 서비스
   - 전체 프로세스 오케스트레이션
   - Serverless
   - 이벤트 기반 운영 가능
   - 다양한 AWS 서비스 연동 가능
   - 다양한 외부 서비스 연동 가능 (Jenkins 등)
   
