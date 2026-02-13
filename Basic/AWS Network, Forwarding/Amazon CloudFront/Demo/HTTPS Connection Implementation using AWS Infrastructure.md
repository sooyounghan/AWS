-----
### Demo - AWS 인프라를 활용한 인증서 없이 HTTPS 구현하기
-----
1. HTTPS : HTTP 전송 중 데이터 유출을 방지하기 위해 통신을 보안에 추가한 프로토콜
2. 일반적인 HTTPS 제공 방법
   - HTTPS 인증서 구매 / 발급 ($10 정도)
   - 인증서 병합 및 설정
   - 웹 서버에 인증서 설치
   - 웹 서버에 설정 변경
     + HTTPS 적용
     + HTTP 리다이렉션 등

3. AWS 인프라를 활용한 인증서 없이 HTTPS 구현
   - AWS 인프라(CloudFront, ALB)를 활용하여 HTTPS 연결을 제공하는 방식 : 둘 다 Route 53 혹은 다른 방법의 도메인 제공 및 인증 필요
   - 장점
     + 인증서 비용 무료
     + 여러 웹 사이트에 적용 가능하며 대상 / Origin 변경 등에 영향없이 제공 가능
     + 인증서 자동 갱신 등 관리가 쉬움

   - 알아둘 점
     + 조금의 추가 비용 발생
     + 경우에 따라 HTTP 통신이 Public에 노출될 수 있음

4. AWS CloudFront를 활용한 HTTPS 구현
   - 기본적으로 배포(Distribution) 생성 시 CloudFront의 아이디가 포함된 도메인 제공 (예) ```d123456abcdef9.cloudfront.net```)
   - 별도로 보유한 도메인을 부여해 CloudFront와 연결 가능
     + Route 53 / 다른 Register에서 도메인 보유 필요
     + 이 때, 선택적으로 HTTPS를 사용해 CloudFront에 접근하도록 설정 가능 : HTTP / HTTPS 사용, HTTP로 Redirect, HTTPS만 사용 3가지 모드 가능

   - 사용 사례
     + S3 Static Hosting 웹 사이트의 HTTPS 제공
     + 단순한 웹사이트 등의 HTTPS 혹은 ALB 등 Origin을 둘 때, HTTPS를 CloudFront에서 받고 싶은 경우

5. CloudFront의 HTTPS 프로토콜 활용
   - ACM에서 SSL 인증서 발급 또는 Import 필요 (US-EAST-1)
   - 비용이 조금 더 비쌈 (HTTP($0.0090 / 10,000) VS HTTPS($0.0120 / 10,000))
   - 두 가지 모드
     + SNI 지원 클라이언트만 지원 ; 무료, 단 예전 브라우저의 경우 지원하지 않을 수 있음
     + All Client : 모든 클라이언트를 지원하지만 CloudFront에 전용 IP 주소 부여 필요 ($600 / Month)

-----
### AWS Certificate Manager
-----
1. AWS 서비스 및 연결된 내부 로스에 사용할 공인 및 사설 SSL / TLS(Secure Sockets Layer / 전송 계층 보안) 인증서를 손 쉽게 프로비저닝, 관리 및 배포할 수 있도록 지원하는 서비스
2. AWS에서 SSL / HTTPS에 사용하는 인증서를 관리하는 서비스 : 인증서를 발급받거나 Import 가능
3. ALB, CloudFront, API Gateway와 연동하여 쉽게 HTTPS 프로토콜 구현 가능
4. 두 가지 종류
   - 무료 : 단, 인증서는 Export 불가능 (즉, ALB / CloudFront / API Gateway / AWS Configure 등 AWS 서비스만 사용 가능)
   - 유료 : 395일 동안 유효한 인증서 (FQDN 당 $15, Wildcard 당 $149)

-----
### 아키텍쳐 (CloudFront)
------
<div align="center">
<img src="https://github.com/user-attachments/assets/3822a300-8372-459d-b3d1-89cc30b20bf1" />
</div>

-----
### Application Load Balancer를 활용한 HTTPS 제공
----- 
1. Application Load Balancer의 리스너에서 443포트를 받아 처리 : HTTPS 통신을 ALB에서 받아 HTTP 프로토콜로 타겟 그룹에 전달하는 방식
2. ACM으로 SSL 인증서 Import 필요 (ALB가 있는 리전) : 혹은 원한다면 자신이 보유한 인증서 Import 가능
3. 아키텍쳐
<div align="center">
<img src="https://github.com/user-attachments/assets/6c45ac9c-a0d3-4c20-9510-6383366c9c43" />
</div>

-----
### 주의사항
-----
1. 인프라에서 Target / Origin까지 HTTP로 연결 : ALB는 Private 통신이 가능하기에 크게 문제는 없으나 CloudFront의 경우 Public 인터넷 통과
2. 실제 Origin / Target 입장에서는 HTTP로 받기 때문에 HTTPS로 리다이렉션 등의 설정이 있는 경우, 무한 리다이렉트 발생 등의 문제 발생
