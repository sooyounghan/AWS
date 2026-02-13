-----
### CloudFront Anycast Static IP
-----
1. CloudFront의 모든 Edge Location에 접근할 수 있는 단일 IP를 부여하는 기능
   - 기존에는 CloudFront에서 IP 주소는 계속 로테이션
   - Anycast : 같은 IP 주소를 다수 리소스에 접근하고 가장 효율적인 노드가 응답하게 하는 방식

2. 사용 사례
   - 고객사에서 CloudFront로 제공되는 서비스 사용 시 방화벽을 열기 위해 고정 IP를 요청하는 경우
   - IP 관리를 조금 더 쉽게 하고 싶은 경우
   - 특정 트래픽만 분리해서 관리하고 싶은 경우

3. 제약 사항
   - IPv6 사용 불가능
   - Use All Edge Location 설정 필요
   - 월 $3000의 요금 발생
