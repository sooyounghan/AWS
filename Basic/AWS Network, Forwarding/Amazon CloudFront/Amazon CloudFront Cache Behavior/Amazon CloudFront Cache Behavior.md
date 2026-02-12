-----
### Amazon CloudFront Cache Behavior
-----
1. CloudFront의 요청이 어떻게 처리되는지 다양한 설정 모음
2. 경로 패턴 단위로 Behavior 구성
   - 예) ```files/*, files/*.png, *.png```
   - 💡 경로 패턴 목록 중 처음으로 매칭된 패턴의 동작 적용
   - 신규 생성시, ```*```로 고정

3. 주요 구성 내용
   - Origin
   - Viewer 설정
   - Cache Policy, Viewer Response Policy, Lambda@EDGE 연결

-----
### Viewer 설정
-----
1. Viewer 프로토콜 : CloudFront에 접근하는 프로토콜
   - HTTP and HTTPS 접근할지 설정 
   - Redirect HTTP to HTTPS (HTTP로 들어오면 HTTPS로 접근)
   - HTTPS only

2. HTTP Method : GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
3. Viewer 액세스 제한 : Presinged URL / Presigned Cookie로만 접근 가능하도록 할 지 여부

-----
### CloudFront의 Cache Key
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/a629f6de-b19d-40b7-9aeb-6701378fb154" />
</div>

-----
### Policy 설정
-----
1. Cache Policy
   - 어떤 키(HTTP Header, 쿠키, QueryString 등)으로 컨텐츠를 캐시하는지 결정
   - 얼마나 오래 캐시하는지(TTL) 결정
   - 콘텐츠를 어떻게 압축해 저장할 것인지 관련 설정

2. Origin Request Policy
   - Origin에 컨텐츠를 요청할 때 어떤 내용을 전달할 것인지 결정(HTTP Header, QueryString 등)
   - Cache Key로 사용할지 여부와 독립적 설정 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/cf815af4-b7ea-4987-86df-b2aea7d1c898" />
</div>

3. Response Headers Policy : 응답 과정에서 어떤 HTTP Header를 제거하거나 더할지 설정
   - 최대 10개
   - Authorization Header만 따로 불가능
<div align="center">
<img src="https://github.com/user-attachments/assets/daae6063-71fd-41a8-b917-7d3a76730d8c" />
<img src="https://github.com/user-attachments/assets/905a0fa7-c68c-44ea-aae0-7e3fe3710cac" />
</div>

-----
### 기타 설정
-----
1. Field Level Encryption : CloudFront의 콘텐츠를 보호하는 방법
2. Lambda@Edge

-----
### Demo
-----
1. CloudFront - 배포 - 동작 - 기본 동작 존재(```*```)
   - 경로 패턴 지정 가능 (기본 배포는 기본값으로 고정(```*```))
   - 원본 : 어떤 원본을 사용할 것인지 설정
   - 자동으로 객체 압축 : 기본적으로 압축 후 전달할지 여부 설정
   - Viewer 프로토콜 : 실제 Viewer(클라이언트 또는 웹 브라우저)가 어떤 프로토콜을 사용할 것인지 결정 (Redirects HTTP to HTTP : 기본값)
   - 허용된 HTTP 방법 : GET, HEAD (기본값)
   - Viewer 액세스 제한 : 별도의 인증 과정을 거쳐 Viewer 접근 설정
   - 캐시 정책 : CacheOptimized (기본값) (기본값 : 하루 / 최소 : 1초 / 최대 : 1년)
     + Create Policy 생성 가능
   - 원본 요청 정책 : 커스터마이징 가능
   - 추가 설정 - 필드 수준 암호화 : Origin에 보안이 확보된 방법으로 전달
