-----
### VPC 관련 몇 가지 알아둘 점
-----
1. 커스텀 VPC 생성 시 만들어지는 리소스 : 라우팅 테이블, 기본 NACL, 기본 보안 그룹
   - 서브넷 생성 시, 모두 기본 라우팅 테이블로 자동 연동
2. VPC에는 단 하나의 Internet Gateway만 생성 가능
   - Internet Gateway 생성 후 직접 VPC에 연동 필요
   - Internet Gateway는 자체적으로 고가용성 / 장애 내구성 확보
3. 보안 그룹은 VPC 단위
4. 서브넷은 가용 영역 단위 (1 서브넷 = 1 가용영역)
5. 가장 작은 서브넷 단위는 /28 (11개, 16 - 5)

-----
### VPC 직접 구성하기
-----
: VPC 생성 방법
   - VPC만 생성
   - VPC와 다른 리소스를 같이 생성 : VPC, 서브넷, 라우팅 테이블, 네트워크 연결 등 쉽게 생성

-----
### VPC 생성 실습
-----
1. 직접 커스텀 VPC 생성 (```10.0.0.0/16```)
2. 서브넷 3개 생성 : Public, Web, DB
3. 각 서브넷에 각 Routing Table 연동
4. Public Subnet에 인터넷 경로 구성 : Internet Gateway
5. Public Subnet에 EC2 프로비전
6. Web Subnet에 EC2 프로비전
   - Bastion Host
   - 해당 EC2에 웹 서버 설치 : NAT Gateway Setup
   - curl로 조회 테스트
7. DB Subnet에 DB 설정 후 Public EC2에 접근 (Optional)
<div align="center">
<img src="https://github.com/user-attachments/assets/7fdec871-6c7f-4576-bc92-cecfcebb7807" />
<img src="https://github.com/user-attachments/assets/9516e467-053c-46c9-940c-fbfadd63f8c5" />
<img src="https://github.com/user-attachments/assets/d16ef465-3d17-4e14-bfd0-ecd29baef794" />
<img src="https://github.com/user-attachments/assets/11fed9f1-13fd-41fb-90e5-156822924dd1" />
<img src="https://github.com/user-attachments/assets/7a5d0bbc-d0ab-404b-8525-645706b4beff" />
</div>
