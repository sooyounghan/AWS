-----
### AWS CodeBuild
-----
1. 빌드와 테스트를 서비스로 제공
   - 요청에 따라 컨테이너로 구성한 빌드 / 테스트 환경을 하나 생성
   - 이후 받을 Source를 기반으로 빌드 / 테스트를 수행해주는 서비스

2. 환경 구성 커스터마이징 가능 : 도커 이미지 / CPU, Memory / 환경 변수 / 제한 시간 / 파일 시스템(EFS) 등 설정 가능
3. AWS IAM 기반으로 다양한 AWS 서비스와 연동
   - 예) CodeCommit / S3 등에서 소스 및 데이터 받아오기
   - 예) CloudWatch와 S3 로그 관리

4. 참고 : Code Lambda 모드 : EC2 대신 Lambda로 Build를 활용하는 모드
5. 설정
   - AMI 이미지 : Amazon Linux, Windows, Ubuntu, MAC OS 등
   - 컴퓨팅 인스턴스 유형 : 미리 설정된 인스턴스의 vCPU / 메모리 등의 구성
     + 선택한 이미지에 따라 선택 가능한 인스턴스 유형이 다름
     + 프로비전 속도 차이 존재 : 수요가 다르기 때문임
   - VPC : VPC 설정 가능 → 상황에 따라 Private Resource에 접근이 필요한 경우
<div align="center">
<img src="https://github.com/user-attachments/assets/7926f0cc-355a-4d2f-97ad-6b719cdc3c61" />

<img src="https://github.com/user-attachments/assets/e47b3612-a6f0-4b3f-a87f-09466814d5d5" />
</div>

6. 프로비전 모드
   - On-Demand : 리소스가 필요할 때 준비해서 사용 후 종료
     + 프로비전 시간 필요
     + MacOS와 MS Server 2022 사용 불가능

   - Reserved Capacity Fleet : 지정한 EC2를 지명하여 준비시키고 사용 후 내가 직접 종료
     + 처음 준비기간 이후 계속 환경이 준비되어 있어서 빌드가 빠름
     + MacOS 사용 가능
     + 배치 빌드 불가능
     + 💡 서울 리전 사용 불가능
<div align="center">
<img src="https://github.com/user-attachments/assets/fd61f9d4-cecc-4c93-b6c3-cce5398888b0" />
</div>

7. buildspec.yml
   - CodeBuild에서 수행 할 내용을 정의한 문서
   - 반드시 소스의 루트 디렉토리에 위치해야 함
   - 기본 이름 : buildspec.yml (CodeBuild에서 변경 가능) (예) buildspec_debug.yml / buildspec_prod.yml)
   - 정의 가능한 내용
     + 빌드 환경(예) node.js, Python 등), 환경 변수, 캐시
     + 스테이지 별 명령어 : install, pre_build, build, post_build
     + 아티팩트 설정 : path 및 구성 방법
     + 기타 (cahce, proxy 등)

   - Phases
     + install : 빌드 / 테스트 환경을 구성하기 위한 패키지 다운로드 및 인스톨
     + pre_build : 빌드 / 테스트 전 수행해야 하는 내용 (Dependency 다운로드 / Install, 외부 리소스 확보 등)
     + build : 실제 빌드 / 테스트 수행
     + post_build : 빌드 / 테스트 이후 마무리 작업 (도커 이미지 Push, Slack 알람, 기록 작성 등)

8. Caching
   - Amazon S3 Caching : 서로 공유할 수 있는 S3 버킷에 내용을 캐시
     + 주로 작거나 미리 빌드해두면 좋은 패키지 등을 저장
     + 네트워크를 사용하므로 큰 파일의 경우 비효율적

   - Local Caching : 하나의 빌드 호스트에 로컬로 저장하는 캐시
     + 주로 크거나 바로 필요한 내용 캐싱
     + 빌드가 빈번한 경우
     + 3가지 모드 : Source Cache / Docker Layer Caching / Custom Cache

9. Local Caching
    - Source Cache
      + Primary / Secondary Source의 Git Metadata를 캐시
      + 캐시 생성 이후부터는 커밋의 변화 부분만 가져옴
      + Git 소스 코드 자체가 엄청 큰 상황일 때(예) node_module이 git에 있을 때)

    - Docker Layer Caching
      + Docker Layer를 캐시
      + 큰 도커 이미지를 빌드하거나 가져올 때 (이미지를 가져오기 위한 네트워크 절약 가능)
      + 리눅스 환경만 가능

    - Custom Cache
      + buildspec에서 명시한 디렉토리만 캐시
      + 소스 다운로드 전에 세팅 → 소스에 동일한 파일명이 있다면 덮어씌움

10. 실전에서 사용할 때 생각할 점
    - 프로비전 시간이 오래 걸림
      + 실제 프로비전까지 10분까지 걸리는 경우도 발생
      + 환경 이미지 / 인스턴스 타입에 따라 프로비전 시간이 다름

    - 비용
      + 예) 서울 general1.large(vCPU, 메모리 15GB : $0.02/Min)
        * 5분에 120원
        * 테스트에 따라 20 ~ 30분도 소요 가능 : 800원
        * 하루 5번 하면 4000원
      + 기타 CloudWatch / KMS / S3 비용 등
