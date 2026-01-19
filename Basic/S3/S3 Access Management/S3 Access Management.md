-----
### S3 권한 관리 방법
-----
1. IAM 정책 : 자격증명(IAM 사용자, 그룹, 역할 등 사용하는 주체) 등 부여하는 정책으로 S3에 대한 권한 부여 / 거부
2. 버킷 정책 : 버킷 자체에 특정 주체가 행사할 수 있는 권한 부여 / 거부 (주체 : IAM 사용자, 역할 등)
3. ACL : 잘 사용되지 않는 추세

-----
### S3의 계층 구조
-----
1. AWS 콘솔에서는 S3의 디렉토리(폴더)를 생성 가능하고 확인 가능
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
   - IAM 사용자 생성 - 사용자 그룹 - demo-user-group
   - 권한 - 권한 추가 - 인라인 정책 생성 - 서비스 : S3 - 나열 : ListAllMyBuckets / 읽기 : GetBucketLocation
   - 리소스 : 모든 리소스
   - 정책 이름 : allow-s3
   - 사용자 : alice (AWS Management Console에 대한 사용자 액세스 권한 제공 – 선택 사항)
     + 사용자 지정 암호 지정
     + 사용자는 다음 로그인 시 새 암호를 생성해야 합니다 설정 해제
     + 사용자 그룹 지정 : demo-user-group
   - 사용자 : bob (AWS Management Console에 대한 사용자 액세스 권한 제공 – 선택 사항)
     + 사용자 지정 암호 지정
     + 사용자는 다음 로그인 시 새 암호를 생성해야 합니다 설정 해제
     + 사용자 그룹 지정 : demo-user-group

2. IAM 사용자에 S3 접근을 허용하는 권한 부여 (IAM 정책)
   - S3 버킷 생성 : demo-my-test-bucket-{계정 ID}
   - IAM 사용자 관련 - 보안 자격 증명 - 콘솔 로그인 - 콘솔 로그인 링크 사용
   - S3 버킷만 확인 가능
   - demo-user-group 사용자 그룹 : AmazonS3FullAccess 권한 추가 (admin)
   - alice로 접근해 확인
   - demo-user-group 사용자 그룹 : 기존 권한 삭제 후 AmazonS3ReadOnlyAccess 변경 후 확인

3. IAM 사용자에 대한 접근을 허가하는 버킷 정책을 만들어 S3에 붙여 접근 허용 (버킷 정책)
   - S3 - 범용 버킷 - 버킷 만들기 - demo-my-home-bucket-{계정 ID}
   - 이 버킷의 퍼블릭 액세스 차단 설정 해제 (모든 퍼블릭 액세스 차단을 비활성화하면 이 버킷과 그 안에 포함된 객체가 퍼블릭 상태가 될 수 있음)
     + 권한 - 버킷 정책 편집 - 변경사항 저장
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListSpecificBucket",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::{버킷명}",
            "Condition": {
                "StringLike": {
                    "s3:prefix": [
                        "",
                        "home/",
                        "home/${aws:username}/*"
                    ]
                }
            }
        },
        {
            "Sid": "FullAccessToUserHome",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::{버킷명}/home/${aws:username}",
                "arn:aws:s3:::{버킷명}/home/${aws:username}/*"
            ]
        }
    ]
}
```
   - 디렉토리 생성 : home
   - home 폴더 내부에 alice, bob 디렉토리 생성 후 alice로 접근하여 alice 디렉토리 및 bob 디렉토리 권한 확인 : alice는 자유롭게 사용 가능, bob은 불가

4. 리소스 정리
   - 버킷 삭제 : 비어있음 또는 삭제 
