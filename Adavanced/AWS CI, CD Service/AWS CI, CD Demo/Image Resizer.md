-----
### Demo - 이미지 리사이저
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/fcde1d31-8db2-4f97-a250-a86974e73875" />
</div>

1. 이미지를 업로드하면 지정한 사이즈로 리사이징 후 다운로드 할 수 있는 웹 사이트
2. Backend
   - Node.js 기반 AWS의 2-Tier 아키텍쳐(ALB-ASG-EC2)로 호스팅
   - CI / CD 파이프라인으로 소스 업데이트 → 빌드 → 배포 과정 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/2404148e-5468-4baa-b3b9-1a7fdc374e1e" />
</div>

3. Frontend
   - Next.js 기반 Static 페이지로 S3로 호스팅
   - CI / CD 파이프라인 기반으로 소스 업데이트 → 빌드 → 배포 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/115ace33-763b-483c-ac46-a7e895ca4ae8" />
</div>

4. 소스 : Git 활용

5. demo-image-resize-on-aws-backend 프로비저닝
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : cloudformation.yml - demo-image-rsz-backend
   - Github 레포지토리 생성 : demo-image-resize-frontend, demo-image-resize-backend
   - CodePipeline - 파이프라인 생성 - Build Custom Pipeline - demo-image-resize-backend - 소스 : Github(버전 2) - Github 연결 / 레포지토리 이름 : demo-image-resize-backend / 기본 브랜치 : prod
     + 빌드 공급자 : Other Build Provider - AWS CodeDeploy - 프로젝트 생성 : demo-image-resize-backend-build - 관리형 이미지 - 서비스 역할 : 새 서비스 역할
       * 환경 변수 설정 가능
       * buildspec 파일 사용 : buildspec.yml
     + 배포 공급자 : AWS CodeDeploy
     + CodeDeploy - 애플리케이션 - 애플리케이션 생성 - demo-image-resize-deploy - EC2/온프레미스
       * 배포 그룹 생성 - demo-my-asg / 서비스 역할 : demp-image-rsz-backend-codedeploy-role / Amazon EC2 Auto Scaling 그룹 : MyASG-demo-image-rsz-backend / 배포 구성 : CodeDeployDefault.AllAtOnce /  로드 밸런싱 활성화 : Application Load Balancer 또는 Network Load Balancer 활성화 후 선택
     + 새 파이프라인 추가 - 배포 공급자 : AWS CodeDeploy / 애플리케이션 이름 : demo-image-resize-deploy

6. demo-image-resize-on-aws-frontend 프로비저닝
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : doc_static_website.yml - demo-image-rsz-frontend / ProjectName : demo-image-rsz, ServiceDomain : image-rsz
   - CodePipeline - 파이프라인 생성 - Build Custom Pipeline - demo-image-resize-frontend - 소스 : Github(버전 2) - Github 연결 / 레포지토리 이름 : demo-image-resize-fronted / 기본 브랜치 : prod
    + 빌드 공급자 : Other Build Provider - AWS CodeDeploy - 프로젝트 생성 : demo-image-resize-frontend-build - 관리형 이미지 - 서비스 역할 : 새 서비스 역할
       * 환경 변수 설정 가능 : SSM_PARAMETER_NAME : SSM - 파라미터 스토어 - demo-image-rsz-backend-ServiceDomain
       * buildspec 파일 사용 : buildspec.yml

7. CodeBuild
   - demo-image-resize-fronted - 편집 - 스테이지 추가 : deploy - 작업 그룹 추가 : deploy / AWS CodeBuild / BuildArtifcat
     + 프로젝트 이름 : demo-image-resize-frontend-deploy
     + 환경변수 : bucket_name / S3우ㅏ image-rsz
     + BuildSepc 이름 : buildspec_deploy.yml
     + 저장

8. Repository에 소스코드 업로드
   - Repository 주소 복사 (프론트엔드, 백엔드 모두 동일)
   - 변경사항 Commit
   - VS Code : Ctrl + Shift + P - Add Remote : my_git
   - Source Control에서 Pull, Push - Push - 인증 : Git에 소스코드 Push
     + Trigger : Branch가 Prod

9. prod(백엔드) 브랜치 생성 후, CodePipeline Trigger
    - prod 브랜치 생성
    - 파이프라인 확인 - 소스 코드 트리거되어 빌드 (Depnedency를 가져온 뒤, Artifact 생성)
    - EC2 - AutoScaling 및 인스턴스 확인 : Allow Traffic을 거치면 완료
    - 이후, 애플리케이션 - 배포 그룹 편집 - 로드 밸런싱 활성화 해제

10. prod(프론트엔드) 브랜치 생성 : 실패
    - 권한 부여 필요 : 파라미터 스토어 및 AWS S3 액세스 필요
      + IAM - 역할 - codebuild 관련 - front-build-service-role에 정책 연결 - AmazonSSMFullAccess
      + front-deploy-service-role에 AmazonS3FullAccess 권한 부여
    - 실행 중지 후, 다시 변경 사항 릴리즈 : 다시 재시작
    - 빌드되어 배포 확인 : S3 - 속성 - Static 웹 사이트 호스팅 엔드포인트로 확인 (파일 선택 후, 이미지 리사이즈 확인 및 서버 메시지 확인 / 편집된 이미지 다운로드 확인)
   
11. 백엔드 수정
    - 배포 메세지 수정 후, Git Commit 후, Push
    - Pull Request (prod에서 main) 후, Merge하면, 파이프라인 실행

12. ASG - 인스턴스 관리 - 수명주기 후기 : EC2가 런칭될 시점 Deploy
    - EC2 인스턴스가 올라가기 전 CodeDeploy가 배포 후, EC2 인스턴스를 올림
    - 즉, 아무것도 없는 채로 트래픽을 받는 경우는 없음
    - 즉, EC2 인스턴스 크기가 늘어나도 자동으로 배포됨


13. 스테이지 추가 - Confirm 스테이지 이름 추가
    - 작업 그룹 추가 - 작업 공급자 : 수동 승인 / 이름 : Confirm
    - 변경 사항 릴리즈 후, 빌드 후, Confirm 과정이 검토를 진행할 수 있음 (승인, 거부)

14. 리소스 삭제
    - CloudFormation - 스택 삭제
    - CodePipeline 삭제
    
