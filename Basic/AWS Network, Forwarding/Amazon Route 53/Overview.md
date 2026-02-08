-----
### Amazon Route 53
-----
1. 높은 가용성과 확장성이 뛰어난 클라우드 Domain Name System (DNS) 웹 서비스
2. DNS : 사람이 읽을 수 있는 도메인 이름(예) ```www.amazon.com```)을 머신이 읽을 수 있는 IP 주소(예) ```192.0.2.44```)로 변환
<div align="center">
<img src="https://github.com/user-attachments/assets/099959d4-cc63-40ec-84c0-8ae41c52c88c" />
<img src="https://github.com/user-attachments/assets/bff7f352-2aba-4f5c-ac06-74f81260008a" />
<img src="https://github.com/user-attachments/assets/0b98ea22-e69f-45c0-9acc-9c0056e14f4b" />
<img src="https://github.com/user-attachments/assets/47ef486d-fc2b-4f28-a8e5-d2ca89b6b676" />
</div>

3. AWS의 DNS 서비스
   - DNS에서 사용하는 포트 숫자 (53)에서 유래
   - 도메인을 IP Address 및 AWS 리소스로 연결해주는 서비스
   - 기본적으로 고가용성을 갖춘 글로벌 서비스 (99.99% SLA)
   - 주요 기능
     + 도메인 관리 (등록, 레코드 연결 등)
     + Health Check 기능 : 주기적으로 지정된 주소에서 정상적인 응답을 받는지 확인
     + 다양한 라우팅 정책
     + 기타 하이브리드 아키텍쳐 환경에서 내부 도메인의 활용 지원
   - 주요 비용 : 호스팅 영역 / DNS 쿼리 / 기타 기능 (Health Check 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/3dc09e9a-1f21-4abc-bc85-a3c078df0dbb" />
</div>
