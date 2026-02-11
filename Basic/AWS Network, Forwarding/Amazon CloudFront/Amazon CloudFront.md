-----
### Amazon CloudFront
-----
1. 개발자 친화적 환경에서 짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상, 애플리케이션 및 API를 전 세계 고객에게 안전하게 전송하는 고속 콘텐츠 전송 네트워크(CDN) 서비스
2. AWS에서 제공하는 Content Delivery Network (CDN)
   - 웹 페이지, 이미지, 동영상 등의 콘텐츠를 본래 서버에서 받아와 캐싱
   - 해당 콘텐츠에 대한 요청이 들어오면 캐싱해둔 콘텐츠를 제공
   - 컨텐츠를 제공하는 서버와 실제 요청 지점 간 지리적 거리가 매우 먼 경우, 요청 지점 근처의 CDN을 통해 빠르게 컨텐츠 제공 가능

3. 엣지 로케이션(Edge Location)에서 데이터를 캐싱 : 따라서 원본이 변해도 캐싱이 만료되지 않는다면 유저가 보는 내용은 변하지 않음
   - 엣지 로케이션(Edge Location) : 따라서 원본이 변해도 캐싱이 만료되지 않는다면 유저가 보는 내용은 변하지 않음
     + AWS CloudFront(CDN) 등의 여러 서비스들을 가장 빠른 속도로 제공(캐싱)하기 위한 거점
     + Global Accelerator와 유저를 연결하는 거점
     + 전 세계에 여러 장소에 흩어져 있음 
<div align="center">
<img src="https://github.com/user-attachments/assets/c72d15ef-7cc8-4980-839a-f8c9f3bf011e" />
<img src="https://github.com/user-attachments/assets/d27b0409-a337-4189-95b9-2b1884548965" />
<img src="https://github.com/user-attachments/assets/314a7000-d16c-4381-ac22-9e9af08b3fac" />
</div>

4. 글로벌 서비스 (단, 리전은 US-East-1 취급)
5. 정적 컨텐츠, 동적 컨텐츠 모두 호스팅 가능
   - 정적 콘텐츠와 동적 콘텐츠
<div align="center">
<img src="https://github.com/user-attachments/assets/5477fbdc-eac7-4ad6-b656-6c9c2962aebc" />
</div>

6. 용어 정리
   - 원본 (Origin) : 실제 콘텐츠가 존재하는 서버 (S3, EC2 등)
   - 배포 (Distribution) : CloudFront의 CDN 구분 단위로 여러 엣지 로케이션으로 구성된 콘텐츠 제공 채널
   - 동작 (Behavior) : 프로토콜, 캐싱 정책, 로그 등 어떻게 콘텐츠를 전달할지 설정
   - Edge Location(Points of Presence (또는 POP) : 데이터를 가장 빠른 속도로 제공(캐싱)하기 위한 거점
   - Regional Edge Cache : Edge Location의 상위 단위로 좀 더 큰 캐싱 거점
   
