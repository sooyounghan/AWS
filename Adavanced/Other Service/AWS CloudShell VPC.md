-----
### AWS CloudShell VPC 모드
-----
1. VPC 모드 : CloudShell을 VPC 안에서 사용할 수 있는 모드
   - Private 서브넷 등에 접근 가능
   - 예) Private EC2 SSH 접속, RDS 접속 등

2. 제한 사항
   - 최대 2개의 환경 구성 / 각 환경별 최대 5개의 보안 그룹 적용 가능
   - 업로드 / 다운로드 불가능
   - 영구 스토리지 활용 불가능 : 세션 종료 후 바로 삭제

3. 인터넷 연결
   - Private Subnet에 위치
   - NAT Gateway 설정이 되어있을 경우 가능

4. 기타 AWS 서비스를 호출하려면 VPC EndPoint 필요

-----
### Demo - CloudShell VPC 모드
-----
1. 신규 VPC 생성
   - VPC 생성 - VPC 등 / demo-cloudshell, VPC 엔드포인트 없음
     
2. Private Subnet에 EC2 생성
   - EC2 생성 : demo-private-ec2 / 키 페어 : demo-my-private-keypair / 네트워크 정보 : demo-cloudshell, 보안 그룹 : 기존 보안 그룹 (default)
   - 보안그룹 : VPC 보안 그룹 - 인바운드 규칙 편집 - 기존 삭세 후, 모든 트래픽, 모든 소스 추
     
3. CloudShell VPC 모드로 EC2 접근
   - 왼쪽 하단 CloudShell 선택
   - ```+``` 선택 후 Create VPC environment
     + demo-my-cloudshell-env
     + VPC는 demo-cloudshell
     + Subnet : Private1
     + 보안그룹 : default

   - 키 페어 복사 후 명령어 입력
```
nano my_keypair.pem
chmod 400 my_keypair.pem
```
   - EC2 인스턴스 주소 (Private IP DNS) 후 명령어 입력하면 EC2 접근 가능
```
ssh -i "my_keypair.pem" ec2-user@{EC2 Private IP DNS}
```

   - 인터넷 사용 : NAT 게이트웨이 프로비전 필요
     + VPC - NAT 게이트웨이 선택 - my-nat-gateway / 가용성 모드 : 영역별 / 서브넷 : demo-cloudshell의 Public1 / 탄력적 IP 할당
     + VPC - 라우팅 테이블 - demo-cloudshell-rtb-private1-ap-northeast-2a - 라우팅 - 라우팅 편집 - 모든 트래픽(```0.0.0.0/0```) / 대상 : NAT 게이트웨이 (my-nat-gateway)
     + CloudShell에서 EC2 접속후 ```ping 0.0.0.0```으로 인터넷 접속 확인

4. 리소스 정리 : EC2 정리 / NAT 게이트웨이 삭제 / EC2 탄력적 IP 주소 릴리
