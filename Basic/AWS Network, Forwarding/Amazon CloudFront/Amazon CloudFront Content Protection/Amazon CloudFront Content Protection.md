-----
### 컨텐츠 접근 제한
-----
1. CloudFront에서 콘텐츠에 대한 접근 제한
   - CloudFront에 접근하는 주체별로 다르게 컨텐츠 접근 제한이 필요한 경우
   - 예) 프리미엄 티어용 영상, 유저 전용 다운로드 이미지 등
   - 두 가지 방법
     + Signed URL : 권한 정보가 담긴 임시 URL을 발급하여 Viewer에게 전달하여 콘텐츠를 다운로드 할 수 있도록 허용 (URL 당 하나의 파일만 사용 가능)
     + Signed Cookie : Viewer가 권한을 행사해 다운로드 할 수 있도록 콘텐츠 접근 권한을 가진 Cookie를 발급해 Viewer에 전달 (다수의 파일에 사용 가능)
     + 만료 기간 설정 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/eef0bf88-f6c7-4670-a81a-5dfc59506e11" />
</div>

2. CloudFront Origin에 대한 직접 접근 제한 : CloudFront를 거치지 않고 직접 Origin에 접근을 막고 싶은 경우

-----
### Signer
-----
1. Signer URL / Cookie를 만들 권한을 가진 주체
2. 두 가지 종류
   - Trusted Key Group (추천) : CloudFront에 Public / Privae Key Pair 중 Public Key를 등록하고, 가지고 있는 Private Key로 Presigned URL / Cookie를 생성하는 방식
   - AWS Account (비추천) : Root 사용자(IAM 사용자 불가능)로 계정의 CloudFront Key Pair를 다운받아 활용 (비추천 : AWS Root 사용자를 활용해야 하며, API를 사용할 수 없고, IAM을 통한 권한 제어 불가능)

3. CloudFront URL을 만들 때, Distribution 단위로 등록된 Key Group 활용 (Key Group : Private / Public Key로 이루어진 키 쌍의 집합으로, CloudFront에 업로드하는 하나 이상의 Public Key로 구성)

----
### Presigned 정책
----
1. Presigned URL / Cookie를 만들 때, URL / Cookie의 권한을 설정하기 위한 정책
2. 두 가지 종류
   - Canned (미리 준비된) Policy : 간단한 버전 / 만료시간만 설정 가능 (단, URL이 짧아짐)
   - Custom Policy : 모든 제약사항 설정 가능 (정책으로 Presinged URL / Cookie의 동작 범위 (만료시간, IP 제한 등) 설정 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/2ff60c7f-1c4b-4e08-adfe-d3c86ba22e1d" />
</div>

-----
### Demo
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/b2a77986-4fb3-4da2-8291-0de8fcfc7807" />
</div>

```
# 데모 프로젝트 적용 순서

1. Public/Private KeyPair 생성 : CloudShell에서 생성
openssl genrsa -out private_key.pem 2048
openssl rsa -pubout -in private_key.pem -out public_key.pem
dir (public_key.pem / private_key.pem)
nano public_key.pem / private_key.pem (확인 가능하며, 이를 복사해서 public.pem / private.pem에 저장)

2. CloudFront에서 Public Key 등록 후 Key Group 생성
   - CloudFront - 퍼블릭 키 - 퍼블릭 키 생성 - demo-my-public-key / Key에 public_key.pem 내용 입력
   - 키 그룹 - demo-my-cf-keygroup - Public Keys에서 Public Key 선택

3. .env 파일에 PUBLIC_KEY_ID, KEYGROUP_ID에 각각 public key id, KeyGroup ID 등록
STAGE=dev
VER=1
PUBLIC_KEY_ID=
KEYGROUP_ID=

4. Node 20이상 설치

5. yarn 설치
npm install yarn -g

6. IAM 프로필 생성
   - Admin - 보안 자격 증명 - 액세스 키 만들기 - 사용 사례 : 기타 - 액세스 키 만들기

7. AWS CLI 설치 / aws configure --profile cf-test (액세스 키 페어 / 리전 입력)

8. 배포
yarn deploy --aws-profile [자신의 프로파일명 = cf-test]
-> 엔드포인트 생성

9. Systems Manager Parameter Store (Default 경로: /demo-cf-preigned -url/dev/private_key/1)에 private key 내용 등록
    - Systems Manager : AWS 다양한 관리와 제어를 하기 위한 서비스들 집합
    - Systems Manager - 파라미터 스토어 - Default 경로: /demo-cf-preigned -url/dev/private_key/1 생성되어 있음 - 편집 - Privatekey 부분에 private key 내용 입력

10. CloudFront - 배포 하나 생성 (demo-cf-presigned-url) - 선택 후 원본 - 원본 액세스 : 원본 액세스 제어 설정
    - 동작 - 뷰어 액세스 제한 : Yes / 신뢰할 수 있는 인증 유형 : Trusted Key Groups (키 그룹이 자동으로 추가되어 있음)

11. S3 버킷(demo-cf-presigned-url-dev-1)에 aws_classroom_square.png 업로드
12. CloudFront Domain/aws_classroom_square.png : 접속 불가 (인증 필요)
13. 프로비전했던 URL 사용 (path와 expire_in 파라미터 필요) : 프로비전했던 URL?path=aws_classroom_square.png&expire_in=300 (300초 동안 유효)
    - URL 출력 : 해당 URL로 접근하면 해당 내용 출력 (즉, Presinged URL을 활용해 CloudFront Distribution에 접근해 이미지를 가져옴)
    - 일반적인 경로로 접근 불가함

14. 프로비전했던 URL?path=aws_classroom_square.png&expire_in=10
   - 10초 이전 : 접근 가능
   - 10초 이후 Access Denied

15. 리소스 정리 : CloudFormation - 스택 선택 후 삭제 / Admin 액세스 키 삭제 (작업 - 삭제 - 비활성화 후 삭제)
```
