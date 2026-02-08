-----
### Amazon Route 53 주요 개념
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/dc4c9672-1e3a-4d06-b95b-97d59047a100" />
</div>

1. 도메인 : 대상의 IP 주소 등의 정보와 매핑되는 사람이 알아볼 수 있는 문자열
   - 서브 도메인 : 도메인 중 스트링 앞에 추가 문자열이 붙은 도메인 (예) ```text.example.com```)
   - APEX 도메인 (Zone Apex, Root Domain, ...) : 도메인 중 앞에 추가 문자열이 없는 순수한 최상위 도메인 (예) ```example.com```)

2. 레코드 (DNS Record) : 도메인이 어떤 방식으로 트래픽을 대상에게 전달하는지 정의하는 데이터
   - 레코드 종류, 대상 IP 주소 등의 정보 포함
   - 레코드 별 TTL : 얼마나 오랫동안 다른 DNS 서버들이 이 레코드를 캐싱할 지 경정 - TTL 값이 높다면 쿼리는 줄어들지만 업데이트 배포 시간 증가
   - Hosted Zone : 레코드의 집합으로 특정 도메인과 서브 도메인의 레코드를 모은 컨테이너 (APEX 도메인과 같은 이름 부여)

3. 레코드의 종류
   - A(Address) Record : 도메인을 IPv4 주소와 연결 (예) ```example.com : 192.33.11.2```)
   - AAAA (IPv6 Address) Record : 도메인을 IPv6 주소와 연결
   - CNAME (Canonical Name) Record : 도메인을 다른 도메인과 연결
     + 예) ```www.example.com``` → ```example.com```
     + 💡 규칙으로 APEX 도메인은 CNAME 사용 불가
   - 💡 Alias (별칭) Record : AWS Rotue53에서만 지원하는 레코드 타입으로 도메인과 AWS 리소스 연결
     + 예) 도메인을 S3 / CloudFront / ALB 등과 연결
     + HTTPS Record : HTTPS를 지원하는 리소스를 위해 더 많은 정보를 제공해 더 효율적인 연결 지원
   - NS(Name Server) Record : 도메인의 Authoritive DNS 서버 지정
   - MX(Mail Exchange) Record : 도메인과 메일 서버 연결
   - TXT(Text) Record : 도메인과 관련된 텍스트 기반 정보 연결

4. 사용 과정
   - 도메인 등록 (Rotue 53 또는 다른 Domain Register) : kr 등의 도메인은 Route 53에서 등록 불가능
   - Hosting Zone
     + Route53에서 도메인을 등록하면 자동으로 Hosting Zone 생성
     + 다른 Domain Register에서 등록했다면, 수동으로 Hosting Zone 생성 후 DNS 연동 필요
   - 레코드 생성
     + AWS 리소스를 연결하려면 Alias Record
     + 기타 필요에 따라 적절한 레코드 타입 생성
     + DNS 캐시 등 이유로 최대 하루 이상 소요

5. Alias Record를 우선적으로 사용하는 이유
   - APEX 도메인 연결 가능 (예) ```awsclassroom.kr```로 ALB 혹은 S3 Static Hosting을 원할 경우 CNAME은 사용 불가능)
   - 무료 : Route53은 쿼리(정보 조회) 당 비용이 발생하나 Alias Record는 무료
   - HTTPS Alias Record는 특히 더 효율적 연결 지원
     + 기존 A / AAAA 레코드의 경우 클라이언트는 첫 요청에서 서버가 지원하는 프로토콜 확인 불가 : 여러 통신 이후 지원 프로토콜을 확인해서 (예) HTTPS / 3) 연결 가능
     + HTTPS Record의 경우 첫 통신에서 지원 프로토콜까지 확인 가능 : 바로 지원하는 프로토콜로 연결 수립 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/560a8a5f-569f-4dfd-8ddb-2f8e9cad8542" />
</div>
