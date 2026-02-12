-----
### 컨텐츠 접근 제한
-----
1. CloudFront에서 콘텐츠에 대한 접근 제한
   - CloudFront에 접근하는 주체별로 다르게 컨텐츠 접근 제한이 필요한 경우
   - 예) 프리미엄 티어용 영상, 유저 전용 다운로드 이미지 등
   - 두 가지 방법
     + Signed URL : 권한 정보가 담긴 임시 URL을 발급하여 Viewer에게 전달하여 콘텐츠를 다운로드 할 수 있도록 허용 (URL 당 하나의 파일만 사용 가능)
     + Signed Cookie : Viewer가 권한을 행사해 다운로드 할 수 있도록 콘텐츠 접근 권한을 가진 Cookie를 발급해 Viewer에 전달 (다수의 파일에 사용 가능)
   - 만료 기간 설정 가능
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
<img src="https://github.com/user-attachments/assets/b2a77986-4fb3-4da2-8291-0de8fcfc7807" />
</div>
