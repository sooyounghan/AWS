-----
### DNS (Domain Name System)
-----
1. DNS (Domain Name System)
<div align="center">
<img src="https://github.com/user-attachments/assets/43cb121c-c2b3-4f55-bd72-915a3695984f" />
<img src="https://github.com/user-attachments/assets/81673c6e-9ef8-4c1e-8087-f35fa7435d5b" />
<img src="https://github.com/user-attachments/assets/b5fbb474-9476-457f-abf0-d0b30e6bf80a" />
</div>

   - 사람이 읽을 수 있는 도메인 이름(예) ```www.amazon.com```)을 컴퓨터가 읽을 수 있는 IP 주소(예) ```192.0.2.44```)로 변환
   - 사람이 읽을 수 있는 문자열과 Internet 프로토콜 기반 정보를 매칭시켜주는 시스템
   - Internet Corporation for Assigned Names and Numbers (ICANN)에서 관리
   - 도메인 (Domain) : 대상의 IP 주소 등의 정보와 매핑되는 사람이 알아볼 수 있는 문자열
     + 서브 도메인 : 도메인 중 스트링 앞에 추가 문자열이 붙은 도메인 (예) ```text.example.com```)
     + APEX 도메인 (Zone Apex, Root Domain, ...) : 도메인 중 앞에 추가 문자열이 없는 순수한 최상위 도메인 (예) ```example.com```)
   - 레코드 (DNS Record) : 도메인이 어떤 방식으로 데이터와 매칭되는지 정의하는 기록
     + 다양한 레코드의 종류가 있으며, 각 다른 정보와 매칭
     + 예) A 레코드는 IPv4 주소, MX 레코드는 메일 서버
<div align="center">
<img src="https://github.com/user-attachments/assets/8dea19c2-3ac1-4d3f-b0ad-40cdb9cad300" />
</div>

   - Domain Zone : 도메인의 정보를 담은 레코드 집합 (예) ```www.awsclassrom.kr / lecture.awsclass.kr``` 등)
   - Zone File : Domain Zone 정보를 저장한 텍스트 파일
   - DNS Query : 주어진 도메인에 해당하는 정보를 요청하는 쿼리
   - Name Server (NS) : DNS Query를 Zone File 기반으로 응답할 수 있는 서버
     + Authoritative : DNS 정보 원본을 가지고 있는 가장 최상위 NS 서버
     + None-Authoritative : Authoritative NS 서버를 조회하여 정보를 보관하고 있거나 응답하는 서버 (캐시)
   - DNS Resolver : 사용자와 NS 서버 사이에 위치한 서버로 실제 유저의 요청에 따라 IP 주소 등의 정보를 확보하는 서버
     + 유저의 클라이언트가 제일 먼저 쿼리를 요청하는 대상이며, 보통 ISP가 관리

2. DNS 규모
   - 2024년 7월 현재 약 3억 6천만개의 도메인 존재 : 모든 HTTP 통신에 매번 활용
   - 매우 큰 규모이며, 이를 호스팅하기 위한 서버 구조에 큰 고민 필요

3. DNS 구성
   - DNS는 계층 구조로, 즉, 최상위 도메인부터 차례대로 계층 구조로 구성
   - 실제 레코드는 가장 마지막 계층에서 보관 및 처리
<div align="center">
<img src="https://github.com/user-attachments/assets/347cebf3-d5fa-4a5f-9d62-78f543095620" />
<img src="https://github.com/user-attachments/assets/df4f4871-f89e-442c-8caf-bf1a225cae4b" />
<img src="https://github.com/user-attachments/assets/5981dc1c-8342-479e-a995-73458ec0d776" />
</div>

4. DNS Root
   - DNS 계층 구조의 최상위 레벨
     + 즉, DNS Query 수행 시 최초로 조회하는 거점
     + 다음 단계인 TLDs(Top Level Domain)의 Zone File을 가진 NS 서버의 주소 정보 보유
   - IANA (Internet Assgined Numbers Authroity)에서 조율하는 13개의 주체에서 관리 : A ~ M까지 각 관리 주체별 다른 서버 주소
   - Root Hints File
     + DNS Root의 주소를 당믄 파일
     + 각 DNS Resolver에 하드 코딩
<div align="center">
<img src="https://github.com/user-attachments/assets/a563018a-e4ea-4694-b08a-570117cccf57" />
<img src="https://github.com/user-attachments/assets/22b09356-ffc6-44f6-87f6-1dc99797cee2" />
</div>

5. Top Level Domain (TLDs)
   - DNS 계층 구조의 두 번째 레벨
   - 실질적으로 정보를 가지고 있는 최상위 레벨 (예) ```.com / .org / .net / .info 등)
   - 종류
     + Country Code TLDs : 각 나라에 할당된 두 자리 코드 (예) .kr / .jp / .uk / .ai 등)
     + Sponsered TLDs : 사설 조직이나 기관에 할당된 TLDs (예) .edu / .gov / .mil 등)
     + New Generic TLDs : 기타 다양한 TLDs (예) .app / .tech / .xyz / .blog 등)
   - 관리 주체 : TLDs Registry (각 회사 / 나라 등)
     + 예) .com / .ent : Verisign, .org : Public Interest Registry, .kr : Korea Internet & Security Agency (KISA)
   - 실제 도메인의 레코드를 관리하는 NS 서버의 주소 정보를 담은 Zone File 보유
<div align="center">
<img src="https://github.com/user-attachments/assets/83cb7c50-53e6-4191-9ff5-aefea05aff55" />
<img src="https://github.com/user-attachments/assets/60cfded1-793c-455f-9651-780c2319342d" />
</div>

6. NS Server
   - 실제 도메인의 레코드를 가지고 있는 서버 : NS 서버의 주소를 TLDs에 등록해두면, 클라이언트에서 DNS 쿼리에 따라 이쪽으로 도착
   - 해당 도메인 및 서브 도메인들이 어떤 프로토콜의 어떤 주소로 매핑되는지(Record)에 관한 Zone File 보유
   - 여기서 최종으로 맵핑된 주소 확보
<div align="center">
<img src="https://github.com/user-attachments/assets/f461bfd1-c96e-4109-a72e-cbb1576fe557" />
<img src="https://github.com/user-attachments/assets/770d3047-8722-4374-bb00-76056e5e0357" />
<img src="https://github.com/user-attachments/assets/3473f139-1404-43a4-a46b-e56d198f8115" />
<img src="https://github.com/user-attachments/assets/11a977d6-c40c-414b-aed2-cfb61aedbc6a" />
<img src="https://github.com/user-attachments/assets/81a7eebb-1054-440b-b0fa-d13e02fc196d" />
<img src="https://github.com/user-attachments/assets/657abfb2-a632-4a53-9fba-98e6df3f10ad" />
<img src="https://github.com/user-attachments/assets/41244589-d7e3-460a-bb17-518739eca3a8" />
</div>

7. 도메인 등록
   - 도메인 등록 대행 (Domain Registrar)을 통해 등록 (도메인 등록 대행 : ICANN에게 인증받고, TLDs Registry와 협의하여 도메인 등록 권한을 가진 주체)
     + 예) 가비아, GoDaddy, Cafe24, AWS Route53 등
   - 등록할 때, TLDs Registry에 자신이 원하는 NS 서버 주소 등록
     + 일종의 남은 슬롯에 자신의 NS 서버를 예치하는 개념
     + NS 서버는 자신의 개인 NS 서버를 사용하거나, DNS Hosting Service 업체에서 대여 가능
     + DNS Hosting Service : DNS 기능을 제공하는 주체
       * 예) 가비아, AWS Route53 등
       * 거의 대부분의 Registrar는 DNS Hosting Service를 같이 제공
<div align="center">
<img src="https://github.com/user-attachments/assets/cc963b73-2d0e-48a3-900a-317b346f5b18" />
<img src="https://github.com/user-attachments/assets/b27c2566-99c8-4373-a632-aa085637c17f" />
<img src="https://github.com/user-attachments/assets/8cda999a-704d-40fa-be3a-0514fede9afa" />
<img src="https://github.com/user-attachments/assets/be4a91d6-568d-4338-8ee8-9ae6a1b43df5" />
</div>

