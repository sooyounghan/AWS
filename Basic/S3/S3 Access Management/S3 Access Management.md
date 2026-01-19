-----
### S3 권한 관리 방법
-----
1. IAM 정책 : 자격증명(IAM 사용자, 그룹, 역할) 등 부여하는 정책으로 S3에 대한 권한 부여 / 거부
2. 버킷 정책 : 버킷 자체에 특정 주체가 행사할 수 있는 권한 부여 / 거부 (주체 : IAM 사용자, 역할 등)
3. ACL : 잘 사용되지 않는 추세

-----
### S3의 계층 구조
-----
1. AWS 코솔에서는 S3의 디렉토리(폴더)를 생성 가능하고 확인 가능
2. S3 내부적으로 계층 구조가 존재하지 않음
   - 키 이름에 포함된 "/"로 계층 구조 표현
   - 예시
     + ```s3://mybucket/world/southkorea/seoul/guro/map.json```
     + 버킷명 : mybucket
     + 키 : world/southkorea/seoul/guro/map.json (단일 스트링)

-----
### S3 버킷 정책
-----
1. 버킷 단위로 부여되는 리소스 기반 정책
2. 해당 버킷의 데이터에 '언제, 어디서, 누가, 어떻게, 무엇을' 할 수 있는지 정의 가능
   - 리소스 계층 구조에 따라 권한 조절 가능
     + 예) resource: ```"arn:aws:s3:::my-bucket/images/*"``` : my-bucket의 images/로 시작하는 모든 객체에 대해
     + 다른 계정 Entity에 대해 권한 설정 가능
     + 익명 사용자 (Anonymous)에 대한 권한 설정 가능
   - 기본적으로 모든 버킷은 Private으로 접근 불가
<div align="center">
<img src="https://github.com/user-attachments/assets/b3441c5c-7038-4621-98fc-671906230deb" />
<img src="https://github.com/user-attachments/assets/e362f4a5-44ab-4057-a6ea-ad4cf9921e20" />
</div>

-----
### S3 버킷 관리 방법 선택
-----
1. IAM 정책
   - 같은 계정의 IAM Entity의 S3 권한을 관리할 때
   - S3 이외의 다른 AWS 서비스와 같이 권한을 관리할 때

2. 버킷 정책
   - 익명 사용자 혹은 다른 계정의 엔티티의 S3 이용 권한을 관리할 때
   - S3만의 권한을 관리할 때

-----
### S3 Access Control List (ACL)
-----
1. 버킷 홋은 객체 단위로 읽기, 쓰기 권한 부여
2. S3에서 설정을 통해 ACL를 활성화시킨 후 적용 가능
3. 파일 업로드 시 설정 가능
4. 간단하고 단순한 권한 관리만 가능
5. 점점 사용하지 않는 추세 : 대부분 경우 버킷 정책 / IAM 정책으로 대체 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/30837a9e-2b7c-4769-8001-97299583ec75" />
</div>

-----
### Demo - S3 권한 부여
-----
1. IAM 정책 / 버킷 정책으로 S3 버킷에 대한 권한 부여
2. IAM 사용자에 S3 접근을 허용하는 권한 부여 (IAM 정책)
3. IAM 사용자에 대한 접근을 허가하는 버킷 정책을 만들어 S3에 붙여 접근 허용 (버킷 정책)

-----
### Demo - S3 홈 디렉토리 만들기
-----
1. IAM 사용자를 생성해 해당 사용자만 접근 가능한 S3 버킷 디렉토리 만들기
