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
<div align="center">
<img src="https://github.com/user-attachments/assets/7fdec871-6c7f-4576-bc92-cecfcebb7807" />
<img src="https://github.com/user-attachments/assets/9516e467-053c-46c9-940c-fbfadd63f8c5" />
<img src="https://github.com/user-attachments/assets/d16ef465-3d17-4e14-bfd0-ecd29baef794" />
<img src="https://github.com/user-attachments/assets/11fed9f1-13fd-41fb-90e5-156822924dd1" />
<img src="https://github.com/user-attachments/assets/7a5d0bbc-d0ab-404b-8525-645706b4beff" />
</div>
   
1. Region 변경 : 도쿄 Region (커스텀 VPC를 만들었을 때, 나중에 리소스 정리가 문제 발생 시,이를 대처하기 위함)
2. 직접 커스텀 VPC 생성 (```10.0.0.0/16```)
    - VPC 생성 (VPC만)
    - demo-my-vpc
    - IPv4 CIDR : 10.0.0.0/16
    - IPv6 : IPv6 CIDR 블록 없음
    - 테넌시 : 기본
    - VPC 생성
    - 보안그룹에 해당 VPC에 해당하는 default 보안 그룹에 이름 부여 : demo-my-vpc-default-sg
    - NACL : demo-my-vpc-nacl
3. 서브넷 3개 생성 : Public, Web, DB
    - 서브넷 - 서브넷 생성 - demo-my-vpc 선택
    - my-public-subnet-01 / my-web-subnet-01 / my-db-subnet-01 : 가용 영역 1a / 1c / 1d 선택
    - IPv4 서브넷 CIDR 등록 : 10.0.0.0/24 / 10.0.1.0/24 / 10.0.1.0/24
    - 서브넷 생성
    - Web, DB 동일
4. 각 서브넷에 각 Routing Table 연동
5. Public Subnet에 인터넷 경로 구성 : Internet Gateway
    - 인터넷 게이트웨이 - 인터넷 게이트웨이 생성 : my-internet-gateway
    - 작업 - VPC에 연결 - demo-my-vpc
    - Routing Table 생성 : 기존 demo-my-vpc의 Routing Table을 my-private-routing-table로 변경
    - 별도 Routing Table 생성 : my-public-routing-table - demo-my-vpc
    - 현재 모두 my-private-routing-table에 연결되어있는데, public subnet만 my-public-routing-table에 연결되도록 설정
      + my-public-routing-table 선택 - 라우팅 - 라우팅 편집
        * 라우팅 추가 : 0.0.0.0/0 - 인터넷 게이트웨이 (my-internet-gateway) - 변경 사항 저장
        * 서브넷 연결 : 명시적 서브넷 연결 - 서브넷 연결 편집 - my-public-subnet-01 선택
        * my-private-routing-table 명시적 연결이 없는 서브넷 2개를 명시적으로 설정
6. Public Subnet에 EC2 프로비전
   - 서브넷 - my-public-subnet-01 - 서브넷 설정 편집 - 퍼블릭IPv4 주소 자동 할당 활성화 활성화
   - demo-my-pulbic-ec2
   - 키페어 생성 : demo-my-vpc-keypair
   - 네트워크 설정
     + VPC : demo-my-vpc / 서브넷 : my-public-subnet-01 / 퍼블릭 IP 자동 할당 활성화 확인
     + 기존 보안 그룹 선택 후, 인스턴스 시작
   - 보안 그룹 : demo-my-vpc-default - 인바운드 규칙 - 인바운드 규칙 편집 - 기존 보안 그룹 삭제 - 규칙 추가 : 모든 트래픽 - 0.0.0.0/0
   - 인스턴스 연결
     + sudo -s
     + dnf install httpd -y (웹 서버 설치)
     + service httpd start
7. 인스턴스 생성 - demo-my-private-ec2
   - 키페어 이용 : demo-my-vpc-keypair
   - 네트워크 설정
     + VPC : demo-my-vpc / 서브넷 : my-web-subnet-01
     + 기존 보안 그룹 선택 후, 인스턴스 시작
8. Web Subnet에 EC2 프로비전
   - Bastion Host
     + demo-my-pulbic-ec2을 demo-my-bastion-host로 변경
     + 다음 명령어 사용
```
sudo -s
nano keyfile.pem (cntrl x, y, 엔터) // keypair.pem 내용
chmod 400 keyfile.pem // 권한 부여
ssh -i "keyfile.pem" ec2-user@instance_ip // private instnace 연결
```
```
sudo -s
dnf install httpd -y // 설치 불가
```
   - 해당 EC2에 웹 서버 설치 : NAT Gateway Setup
      + VPC - NAT 게이트웨이 - NAT 게이트웨이 생성 - my-net-gateway
      + 가용성 모드 - 영역별 - 서브넷 (my-public-subnet-01)
      + 탄력적 IP 할당 ID 할당
      + Routing Table 편집 : my-private-routing-table - 라우팅 편집
        * 라우팅 추가 : 0.0.0.0/0 - NAT 게이트웨이 (my-net-gateway)
```
sudo -s
dnf install httpd -y // 설치 가능
```
   - curl로 조회 테스트
```
curl google.com
```

10. DB Subnet에 DB 설정 후 Public EC2에 접근 (Optional)
    - 인스턴스 시작 : demo-my-db-ec2
    - 키페어 이용 : demo-my-vpc-keypair
    - 네트워크 설정
      + VPC : demo-my-vpc / 서브넷 : my-db-subnet-01 
      + 기존 보안 그룹 선택 후, 인스턴스 시작
    - Cloudshell 이용 : + 선택 - Create VPC enviornment
      + Name : my-vpc-cloudshell
      + VPC : demo-my-vpc
      + Subnet : my-db-subnet-01
      + Security Group : Default
```
sudo -s
nano keyfile.pem (cntrl x, y, 엔터) // keypair.pem 내용
chmod 400 keyfile.pem // 권한 부여
ssh -i "keyfile.pem" ec2-user@instance_ip // private instnace 연결
```
```
sudo -s
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm 
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf install mysql-community-server -y   // 서버 설치
service mysqld start
```
```
grep 'temporary password' /var/log/mysqld.log // 임시 패스워드 확인
mysql -u root -p

ALTER USER 'root'@'localhost' IDENTIFIED BY 'aBcd1234!!'; // 패스워드 변경
CREATE USER 'testuser'@'%' IDENTIFIED BY 'aBcd1234!!';
```
   - demo-my-public-ec2 인스턴스
```
sudo -s

sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm 
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023

sudo dnf install mysql-community-client -y   // 클라이언트 설치

mysql -h {db-ec2 IP 주소} -u testuser -p
```

11. Cloushell - 작업 - 삭제 / EC2 인스턴스 종료 / VPC NAT 게이트웨이 삭제 / 탄력적 IP 삭제 / VPC (demo-my-vpc) 삭제 
