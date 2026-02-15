-----
### Demo - URL Redirector
-----
1. DynamoDB에 URL을 저장하여 URL을 단축시키는 애플리케이션 (Route 53 도메인 등록 필요)
<div align="center">
<img src="https://github.com/user-attachments/assets/d562566f-d05d-4d73-ad15-433e61ef9390" />
<img src="https://github.com/user-attachments/assets/0131037c-2219-4217-a9ca-051a52b8f16f" />
<img src="https://github.com/user-attachments/assets/f8f225e4-2213-4832-9abe-ae895950b9bf" />
</div>

2. S3, DynamoDB
   - demo-ddb-source-{계정ID} 생성 후, src 폴더, .env / package.json / package-lock.json / readme.md 파일 업로드
   - demo-shortlink
     + 파티션 키 : URL
     + 설정 사용자지정 - 보조 인덱스 - 글로벌 인덱스 생성 (파티션 키 : shortlink)

4. demo-url-redirector-backend
   - .env
```
DYNAMODB_TABLE_NAME=demo-shortlink
REGION=ap-northeast-2
```
   - ACM - 요청 - 퍼블릭 인증서 유형 - sl.Rotue53 DNS / *.Route 53 DNS / Route 53 DNS - Route 53에서 레코드 생성 - ARN 복사
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : cloudformation.yml - demo-short-link
     + HostedZoneName : Route 53 DNS
     + MyCertificateArn : ACM ARN
     + ServiceDomain : sl.Route 53 DNS
     + SourceBucket : demo-ddb-source-{계정ID}

5. demo-url-redirector-frontend
```
1. Backend 프로비전
2. .env URL 업데이트
3. Dependency 업데이트
   - yarn, npm 등
   - .env 파일 : NEXT_PUBLIC_SHORTLINK_URL=demo-short-link의 SerivceDomain
4. EC2 인스턴스 : ShortLinkInstance 확인
5. 실행 : yarn dev
```

6. ```http://www.naver.com```에 대한 ShortLink 확인 및 접속 확인
7. DynamoDB - 항목 탐색 - 테이블 - demo-shortlink 항목 확인 / 인덱스 shortlink-index 글로벌 인덱스 사용 확인
8. 리소스 정리 : CloudFormation 스택 삭제 / DynamoDB 테이블 삭제 / S3 버킷 비우기 후 삭제
