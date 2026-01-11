-----
### CIDR (Classless Inter-Domain Routing)
-----
1. Class Inter-Domain Routing
   - IP는 주소의 영역을 여러 네트워크 영역으로 나누기 위해 IP를 묶는 방식
   - 여러 개의 사설망 구축을 위해 망을 나누는 방법

2. CIDR 개념
   - IP 주소의 집합
   - 네트워크 주소와 호스트 주소로 구성
   - 각 호스트 주소 숫자 만큼의 아이피를 가진 네트워크 망 형성 가능
   - A.B.C.D/E 형식
     + 예) ```10.0.1.0/24, 172.16.0.0/12```
     + A, B, C, D : 네트워크 주소 + 호스트 주소 표시
     + E : 0 ~ 32로, 네트워크 주소가 몇 Bit인지 표시
<div align="center">
<img src="https://github.com/user-attachments/assets/68b7d9a8-5e8d-48e9-b3fb-8d2683aa7289" />
<img src="https://github.com/user-attachments/assets/591eba97-2f3a-468f-b614-e71782e781eb" />
</div>

3. CIDR Block
   - 호스트 주소 비트만큼 IP 주소 보유 가능
   - 예) ```192.168.2.0/24```
     + 네트워크 비트 : 24
     + 호스트 주소 = 32 - 24 = 8 : 즉, $2^{8} = 256$개의 IP 주소 보유
     + ```192.168.2.0 ~ 192.168.2.255```까지 총 256개 주소 의미

   - 예) ```10.88.135.144/28```
     + 네트워크 비트 : 28
     + 호스트 주소 = 32 - 28 = 4 : 즉, $2^{4} = 16$개의 IP 주소 보유
<div align="center">
<img src="https://github.com/user-attachments/assets/09f75e5f-e25c-4ec3-b036-8752ba8af633" />
</div>

4. 서브넷
   - 네트워크 안의 네트워크
   - 큰 네트워크를 잘게 쪼갠 단위
   - 일정 IP 주소의 범위 보유 : 큰 네트워크에 부여된 IP 범위를 조금씩 잘라, 작은 단위로 나눈 후 각 서브넷에 할당
<div align="center">
<img src="https://github.com/user-attachments/assets/6eb22be8-0ddb-45cd-a5a9-a88b7d85cfeb" />
</div>

5. AWS 서브넷의 IP 개수
   - AWS의 사용 가능 IP 숫자는 5개를 제외하고 계산
   - 예) ```10.0.0.0/24```
     + ```10.0.0.0``` : 네트워크 어드레스
     + ```10.0.0.1``` : VPC Router
     + ```10.0.0.2``` : DNS Server
     + ```10.0.0.3``` : 미래에 사용을 위해 남겨둠
     + ```10.0.0.255``` (마지막 번호) : 네트워크 브로드캐스트 어드레스 (단, 브로드 캐스트를 지원하지 않음)
     + 따라서, 총 사용 가능한 IP 개수는 $2^{8} - 5 = 251$
