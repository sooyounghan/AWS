-----
### Amazon CloudFront Caching
-----
1. CloudFront의 캐싱 티어 : 2-Tier
   - 요청 순서
     + Edge Location
     + Regional Edge Cache
     + Origin
<div align="center">
<img src="https://github.com/user-attachments/assets/68d4e575-8168-490c-b19f-7f25912ec152" />
<img src="https://github.com/user-attachments/assets/9023f8a5-fd7e-41f4-9dec-edf2c20af239" />
<img src="https://github.com/user-attachments/assets/954481c7-9470-4d76-a02a-de366a039856" />
<img src="https://github.com/user-attachments/assets/fb0fc42b-fe60-44fb-acb7-1fcdf9af2a39" />
<img src="https://github.com/user-attachments/assets/07a22ec3-88f9-4c97-8e53-37fa0ef792a2" />
<img src="https://github.com/user-attachments/assets/6975403b-40d9-48ec-99be-4d86f92f1bdf" />
<img src="https://github.com/user-attachments/assets/30931912-356b-49f9-ab6c-f6078c91ced2" />
<img src="https://github.com/user-attachments/assets/1e5b044d-5289-4380-b7b3-77a66d660d0f" />
<img src="https://github.com/user-attachments/assets/00dfb516-b19d-41e3-94e5-56a7ba01ebed" />
</div>

2. Cache Key
   - 요청에 따라 어떤 캐시 내용을 보여줄지를 결정하는 정보의 조합 : 각 오브젝트는 고유의 Cache Key 단위로 캐시
   - Cache Hit : Viewer가 특정 Cache Key로 오브젝트를 요청하였을 때, Edge Location에서에서 해당 Cache Object를 가지고 있어 원본에 요청 과정 없이 제공할 수 있는 상황
     + Origin의 부하 경감 가능
     + 더 빠르게 컨텐츠 제공 가능
     + 즉, CDN이 Cache Hit이 많을수록 더 좋은 퍼포먼스 제공 가능
   - 주요 Cache Key 구성 요소 : 경로(기본), Query String, HTTP Header, Cookie
<div align="center">
<img src="https://github.com/user-attachments/assets/fd1e26b8-805a-4868-b796-534ef433576b" />
<img src="https://github.com/user-attachments/assets/9854a187-2c4d-4ecb-a6bd-cd1d22294635" />
<img src="https://github.com/user-attachments/assets/07021218-3c1e-4c95-807d-97c7b950649a" />
<img src="https://github.com/user-attachments/assets/ddb94883-df25-498b-87f5-5b134e8456e0" />
<img src="https://github.com/user-attachments/assets/dd8abe15-cc9b-46b5-9459-65c0a9285889" />
<img src="https://github.com/user-attachments/assets/ef7a3fa5-6d3b-4bba-b2f2-b5e437673504" />
<img src="https://github.com/user-attachments/assets/6b395213-508a-46b1-94a7-e1ddff74bada" />
<img src="https://github.com/user-attachments/assets/1bf8f24f-e282-404e-aac1-79c5c89944bf" />
<img src="https://github.com/user-attachments/assets/52509aa7-616c-4d40-b768-b95be45a22be" />
<img src="https://github.com/user-attachments/assets/dab36fb8-1376-43b7-8081-1f5f9d606a4c" />
</div>

3. HTTP Header based Cache
   - Cache Key 중 HTTP Header 활용
   - 활용 사례
     + 언어별 캐싱
     + Device Type별로 캐싱
       * CloudFront에서 별도로 User-Agent를 기반으로 전용 헤더 생성
       * CloudFront-Is-Desktop-Viewer / CloudFront-Is-Mobile-Viewer / CloudFront-Is-SmartTV-Viewer 등

   - 지역별 캐싱 : CloudFront에서 전용 헤더 생성 : CloudFront-Viewer-Country
   - 헤더 명은 대소문자를 구분하지 않지만, 값은 구분

4. Cookie based Cache
   - Cookie를 기반으로 컨텐츠 내용 캐시 : Cookie를 활용하지 않은 HTTP 서버 혹은 S3에서 사용할 경우 퍼포먼스만 저하될 수 있음
   - Cache 만료 : 캐시된 Object는 일정 기간(TTL, Time To Live) 이후 만료
     + 그 다음 요청이 올 경우 CloudFront는 Origin에 Object 갱신 여부 확인
     + Origin이 304 Not Modified를 줄 경우 갱신 필요 없음
     + 200 OK와 파일을 줄 경우, 갱신
<div align="center">
<img src="https://github.com/user-attachments/assets/ef7c1a4a-cb4e-47ee-b4db-24b0dbd67b09" />
<img src="https://github.com/user-attachments/assets/4a6fdc02-6556-4c0b-afef-b6f4ca88d21e" />
</div>

5. Cache TTL
   - TTL (Time To Live) : Cache Object를 얼마나 오래 보관할지에 관한 설정
     + 기본 24시간
     + 모든 CloudFront의 Object에 적용
     + 파일 단위에서는 Origin에서 Cache-Contorl 헤더 혹은 Express 헤더를 포함해서 조절 가능

   - TTL 종류
     + Miminum TTL : 최소 TTL, 즉, 파일 단위 컨트롤에서 줄 수 있는 최소 TTL
     + Maximum TTL : 최대 TTL, 즉, 파일 단위 컨트롤에서 줄 수 있는 최대 TTL
     + Default TTL : 별도의 설정이 없을 경우 부여되는 기본 TTL

6. Cache TTL 컨트롤
   - 파일 단위에서는 Origin에서 Cache-Contorl 헤더 혹은 Expires 헤더를 포함해서 조절 가능
   - Cache-Control : 얼마나 오래 Object를 Cache 하는지 기간 설정
     + max-age : CloudFront와 브라우저 둘 다 영향
     + s-maxage : CloudFront만 영향
     + no-cache, no-store : 캐싱하지 않음 (단, Min TTL이 0 이상일 경우 Min TTL로 최저 설정)
   - Expires : Cache가 만료되는 정확한 시각 설정 (CloudFront와 브라우저 영향)
   - CloudFront의 Min / Max TTL 범위 안에서만 설정 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/88fae641-40f6-4587-b0f0-6d78c79af219" />
</div>

7. Cache Policy
   - 캐싱과 관련된 내용을 정책으로 정의하여 CloudFront에 적용 가능
   - 주요 설정
     + 어떤 키(HTTP Header, 쿠키, QueryString 등)으로 컨텐츠를 캐시하는지 설정
     + 얼마나 오래 캐시하는지(TTL) 설정
     + 컨텐츠를 압축 저장 관련 설정

   - 두 가지 종류
     + Managed : AWS에서 직접 생성한 Policy로 다양한 상황을 위해 미리 준비된 Policy
     + Custom : 직접 설정
