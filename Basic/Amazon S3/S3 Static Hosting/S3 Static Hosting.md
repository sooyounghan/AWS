-----
### 정적 콘텐츠와 동적 콘테츠
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/c799d228-1828-458f-ba65-afebc5f52189" />
</div>

-----
### S3 정적 호스팅
-----
1. S3을 사용해 정적(Static) 웹 컨텐츠를 호스팅하는 기능
2. 별도의 서버 없이 웹사이트 호스팅 가능
3. 사용 사례 : 대규모 접속이 예상되는 사전 예약 페이지, 홍보 페이지, 회사 웹 사이트 등
4. 장점
   - 고가용성 / 장애 내구성 확보
   - Serverless
     + 즉, 사용한 만큼만 비용 지불
     + 배포와 수정이 쉬움
   - 자체적으로 도메인 주소 변경, HTTPS 불가능
     + 단, Rotue53 / CloudFront 등과 연동하여 주소 변경 및 HTTPS 구현 가능
     + 주의 : Route53 레코드로 Static Hosting을 연결하려면, 버킷명과 도메인 명이 같아야 함

-----
### 실습
-----
1. S3 버킷의 정적 호스팅 기능 활성화하기
   - demo-static-hosting-{계정ID} 버킷 생성
   - 모든 퍼블릭 액세스 차단 해제
   - 권한 - 버킷 정책
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::[버킷명]/*"
        }
    ]
}
```

2. S3 버킷에 Static Contents 파일을 작성해 업로드하기
3. S3 정적 호스팅 웹 주소를 사용해 웹 사이트 동작 확인하기
  - 속성 - 정적 웹 사이트 호스팅 - 활성화
    + 인덱스 문서 : index.html
    + startbootstrap-freelancer-gh-pages 내 폴더 및 파일 업로드
    + 속성 - 정적 웹 사이트 엔드포인트로 접속
    + index.html 파일 변경 후 재업로드 후 확인

4. 리소스 정리 : 버킷 삭제
