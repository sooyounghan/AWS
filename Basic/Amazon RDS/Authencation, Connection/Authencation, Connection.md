-----
### Amazon RDS 접속
-----
1. RDS 생성 시 두 종류의 IP가 할당
   - Private IP : 기본적으로 할당
     + VPC 내부 리소스가 RDS에 접근하기 위해 사용
     + RDS의 DB Instance가 위치한 서브넷에 따라 Range 결정
   - Public IP : 퍼블릭 접근 가능 옵션을 선택했을 경우 할당
     + Private 서브넷에 DB Instance가 있을 경우 할당되지 앟음

2. RDS의 IP는 다양한 상황에서 변경
   - 중지 → 재시작 / DB 인스턴스 교체 / AWS에서 점검 / OS 패치 / DB 엔진 버전 업데이트 등
   - 따라서, 가능하면 DNS로 접근하는 것을 권장

3. 일반적으로 Production DB는 프라이빗 서브넷에 두는 경우가 많음 : 보안적으로 뛰어남
4. 접속 : Bastion Host로 가능
   - Instance Connect Endpoint가 있을 경우 활용 가능 (무료) : 3389 포트만 활용 가능, 즉, RDS를 3389로만 사용해야 함
   - VPN / Direct Connect 등으로 VPC와 연결
<div align="center">
<img src="https://github.com/user-attachments/assets/54ab8862-a718-4064-9fec-4796f02bc97c" />
</div>

-----
### Amazon RDS 인증
-----
1. 일반적인 Username / Password : AWS Secrets Manager로 관리 가능
2. IAM 인증
   - IAM을 활용해 임시 토큰을 생성하여 (15분) RDS에 접속하는 방법 : 이 때, IAM 인증을 허용해줄 유저를 DB에 넣어줘야 함
   - 토큰 생성을 위해서는 rds-db:connect 권한 필요
   - IAM 컨디션 활용 가능 (예) 인턴은 type:devonly 태그가 붙은 RDS 인스턴스에 회사 아이피로 1월 1일부터 1월 16일까지 아침 8시부터 저녁 6시까지만 접속 가능)

3. Kerberos

-----
### Demo - Amazon RDS 인증과 접속
-----
1. 목표
   - 프라이빗 서브넷에 생성된 RDS에 Bastion Host를 만들어서 접속
   - RDS에 IAM DB 인증을 활용하여 접속

2. 기본 VPC에 두 개의 프라이빗 서브넷 생성 : 라우트 테이블 역시 생성
3. 프라이빗 서브넷에 RDS를 프로비전
4. Bastion Host를 통해 RDS 접속 확인
   - ID / Password
   - IAM DB 인증 토큰으로 RDS 접속 확인
<div align="center">
<img src="https://github.com/user-attachments/assets/08a769aa-84a3-457d-9477-ea425fb5f3fa" />
<img src="https://github.com/user-attachments/assets/fce92b1c-0ff4-4a81-870a-8e49c4fcf91e" />
</div>

 
