-----
### AWS CodePipeLine 주요 개념
-----
<div align="center">
<img  src="https://github.com/user-attachments/assets/583bd11d-486a-4122-82c5-8985f8ebbd87" />
</div>

1. 파이프라인(Pipeline) : 여러 배포 과정(Stage)로 구성된 워크플로우
2. 스테이지(Stage) : 격리된 환경에서 작업을 수행하는 논리적 단위
   - 다양한 작업(Action)으로 구성
   - 아티팩트(Artifact)를 받아서 아티팩트 기반으로 정해진 작업을 수행
   - 제한 없는 숫자로 동시에 다양한 작업을 수행 가능

3. 작업(Action) : 각 스테이지에서 수행하는 작업 (예) 소스 가져오기, 빌드, 테스트, 배포, 인가 대기 등)
4. 아티팩트(Artifact) : 각 스테이지에서 Input으로 제공되는 데이터
   - Code, Dependency 파일, Template 등
   - Input Artifact : 각 스테이지에서 작업에 사용하는 기반 파일
   - Output Artifcat : 스테이지의 작업 결과물로 다른 스테이지로 전달하거나 배포되는 파일
   - S3에 저장

5. Transitions : 각 스테이지 간 이동 포인트 (비활성화 가능)
6. 소스 연결(Connection) : 외부의 소스 레포지토리와 연결하는 단위 (Bitbucket, Github Enterprise Server, GitLab, GitLab Self-Managed 지원)
<div align="center">
<img src="https://github.com/user-attachments/assets/883cd089-0b98-4fb5-8cd5-4f9daa48a9fe" />
</div>

-----
### Demo
-----
1. GitHub 레포지토리 생성
   - demo-codepipeline
   - Create New File : test.js 붙여넣기 후, test.js - Commit Change

2. CodePipeline 생성
   - CodePipeline - 파이프라인 생성 - Custom 기반 생성
   - demo-my-codepipeline
   - 실행 모드 : 대기
   - 역할 이름 : 유지 (역할 생성 - 기본값)
   - 소스 : Github (버전 2)
     + 연결 : Github 연결 - 연결 이름 : demo-my-github-connection
     + 앱 설치 가능 (GitHub와 AWS 코드 시리즈를 연결) - 새 앱 설치 - All Repository - 연결
     + 레포지토리 : demo-my-codepipeline
     + 기본 브랜치 : main
     + Start your pipeline on push and pull request events : Main 브랜치 한정 해당 이벤트 발생하면 파이프라인 수행
       * 이벤트 유형 선택 가능
     + 필드 : Commands (노드 설치 후 실행) / 배포 제외 / 파이프라인 생성
```
echo "installing node..."
n 20.10.0
node test.js
```

4. 테스트 node.js 스크립트 생성

5. CodePipeline 파이프라인 생성 : 내용을 출력하는 스크립트 테스트
   - 빌드 - 세부 작업 내용 보기 - 로그 출력
   - 완료되면 Success
   - 소소 코드 변경 : Hello World, Hello AWS, Hello Lambda로 변경 (Commit Change - 이벤트 발생)
     + 빌드 재실행
   - 편집 : 트리거 / 소스 / 빌드 스테이지 편집 가능 (유연한 파이프라인 수행 가능)

6. 리소스 정리 - CodePipeline 삭제 / IAM 역할 삭제
