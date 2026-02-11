-----
### Amazon CloudFront Origin
-----
1. 원본 (Origin) : Viewer에게 보여줄 콘텐츠의 원본이 있는 거점
   - 두 가지 종류
     + Amazon S3
     + Custom Origin
       * EC2
       * ELB
       * 기타 EC2 및 다른 HTTP 서비스
   - 기본 : 최대 25 / Distribution
<div align="center">
<img src="https://github.com/user-attachments/assets/2bae8798-8708-4e9a-b791-b4d8f0ddf30b" />
</div>

2. S3 Origin
   - Amazon S3을 Origin으로 설정해 콘텐츠를 제공하는 경우
   - 도메인 형식 : ```{bucketname}.s3.{region}.amazonaws.com```
     + 이렇게 설정하지 않을 경우 : Custom Origin 취급
       * ```s3.amazonaws.com/{bucketname}``` (X)
       * ```http://{bucketname}.s3-website-{region}.amazonaws.com``` (가능, 단 S3 Static Hosting)

   - S3 Origin 만의 추가 기능
     + S3의 접근 제한 가능 (OAC / OAI)
     + POST / PUT 등으로 직접 S3에 컨텐츠 업데이트 가능

   - 기타 활용 : S3 Object Lambda

3. Custom Origin
   - S3을 제외한 모든 Origin
     + MediaStore
     + S3 Static Hosting
     + Lambda Function URL
     + Application Load Balancer
     + EC2 또는 HTTP Source

   - Origin Group
     + 여러 Origin을 그룹으로 묶어 Failover 시나리오에 대응 가능
     + 예) Primary에서 HTTP Status 500을 반환할 경우, Secondary에서 콘텐츠 가져오기

   - HTTP / HTTPS로 접근할지 선택 가능
   - IP 주소는 사용 불가 : 도메인만 가능

4. Origin Group
   - Failover를 대비하여 Primary, Secondary 두 Origin 그룹으로 묶어 관리 가능
   - Primary에서 실패한 경우 자동으로 Secondary에 요청
     + 실패를 나타내는 HTTP 코드가 반환된 경우
     + Primary에 통신을 할 수 없는 경우 (타임아웃 등)
       * 기본 10초 (3번 시도)
       * 시도 횟수와 시간 조절 가능
     + 요청의 응답이 늦어지는 경우 : 기본 30, 최대 60초 조절 가능
   - GET / HEAD / OPTION Method에만 적용
   - Primary / Secondary 모두 실패한 경우 커스텀 에러페이지 생성 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/531276eb-1ae6-4aea-a278-a819a1a1e77a" />
</div>

5. Origin Custom Header : CloudFront에서 Origin에 요청 시 커스텀 추가 헤더 전달 가능
   - 이미 요청에 포함되어 있으면 덮어씌움
   - 최대 10개 (증가 요청 가능)
   - 별도로 추가 불가능한 헤더 존재
     + 예) Host, Range, Connection, Cache-Contorl, X-Amz-* 등
<div align="center">
<img src="https://github.com/user-attachments/assets/527c4c37-3137-4385-9cc7-316ea8446ee9" />
</div>

