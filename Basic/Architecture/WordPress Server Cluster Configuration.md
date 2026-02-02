-----
### 3-Tier Architecture
-----
1. 정형화된 3개의 물리적 / 논리적 티어로 구성된 애플리케이션 아키텍쳐
2. 일반적인 티어
   - Presentation Tier : 사용자와 직접 소통하는 티어 (주로 HTML, CSS와 같은 유저를 위한 UI로 구성)
   - Application Tier : 실제 로직을 처리하는 티어 (프로그래밍 언어(PHP, Java, Python) 등으로 구성)
   - Data Tier : 데이터를 저장하는 티어 (데이터베이스 혹은 데이터를 처리하는 저장 공간으로 구성)

3. ALB / WEB EC2 / RDS 3가지 티어로 구성

-----
### Demo
-----
1. 3-Tier Architecture 아키텍쳐를 활용해 WordPress 서버를 위한 웹 서버 클러스터 구성
2. EC2 AutoScaling과 ELB로 고가용성 확보
3. EFS를 활용해 공유 스토리지에 워드프레스 설치
<div align="center">
<img src="https://github.com/user-attachments/assets/a79e2f85-9e9b-4c26-a750-2c3988ca6134" />
<img src="https://github.com/user-attachments/assets/cdd16750-a44b-47a4-8ce8-9979ab939444" />
</div>

4. 순서
<div align="center">
<img src="https://github.com/user-attachments/assets/1ee118ac-c31c-43a5-a0ae-7f78cdfb0c84" />
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/1e3ed177-adb3-4bbf-b4f9-30fbfd31167b" />
</div>

   - IAM 역할 생성
     + EC2 - AmazonS3FullAccess - demo-3-tier-ec2-role

   - VPC 생성 : Public / Private Subent 각 2개
     + VPC 등 - demo-3-tier - VPC 엔드포인트 없음
     + 생성 VPC 보안 그룹 : 인바운드 규칙 - 규칙 제거 후 규칙 추가 : 모든 트래픽 및 이름 변경 : demo-3-tier-default
       
   - RDS 서브넷 그룹 생성 : RDS 프로비전
     + 서브넷 그룹 : demo-3-tier-rds-subnet-group
     + VPC : demo-3-tier
     + 가용 영역 : 2a, 2b, private subnet
     + RDS 프로비전 : MySQL, 프리티어 / demo-3-tier-db / 마스터 암호 설정 / VPC : demo-3-tier / 추가 구성 - 초기 데이터베이스 이름 : wordpress
      
   - EFS 생성
     + demo-3-tier-efs / demo-3-tier-vpc
     + 보안 그룹 : default 보안 그룹으로 설정

   - S3 버킷 생성 : wp-config 업데이트 및 업로드
     + demo-3-tier-soruce-bucket-{계정ID}
     + demo_3_tier_userdata (변경 : efs_id (EFS의 파일 시스템 ID), S3 버킷)
     + wp-config : 워드프레스가 사용하는 데이터베이스 등 설정 (변경 : 데이터베이스 엔드포인트 주소) 후 업로드
```php
/** Database hostname */
define( 'DB_HOST', 'demo-3-tier-db.clamcm2mun2a.ap-northeast-2.rds.amazonaws.com');
```

   - 유저 데이터 준비 및 EC2 생성 : 해당 EC2로 WordPress 생성 - AMI
     + demo-3-tier-ec2
     + 키 페어 사용
     + demo-3-tier-vpc / public 서브넷 선택 / 퍼블릭 IP 자동 할당 활성화 / 기존 보안 그룹 (default) 선택
     + IAM 인스턴스 프로파일 : demo-3-tier-ec2-role
     + 유저데이터 입력
```
#!/bin/bash
dnf install httpd -y
dnf install -y php8.2 php8.2-mysqlnd mariadb105
systemctl restart httpd
chkconfig httpd on
chown -R ec2-user:ec2-user /var/www/html
mkdir /var/www/html/wordpress
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
echo "$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" "http://169.254.169.254/latest/meta-data/placement/availability-zone").{efs_id}.efs.ap-northeast-2.amazonaws.com:/    /var/www/html/wordpress nfs4    defaults" >> /etc/fstab
mount -a
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
cp wordpress /var/www/html -r 
chown  ec2-user /var/www/html/wordpress
chmod -R o+r /var/www/html/wordpress
aws s3 cp s3://{s3_버킷}/wp-config.php  /var/www/html/wordpress --region ap-northeast-2  
```
```
sudo -s
cd /var/www/html
```
   - {Public_IP}/wordpress 연결
   - English로 진입
     + Title : demo-3-tier
     + Username : admin
     + Password
     + Email
   
   - 해당 AMI로 Launch Template : AutoScaling Group + ALB
     + ALB : 로드 밸런싱 - 대상 유형 : 인스턴스 / 대상그룹 : demo-3-tier-target-group / VPC : demo-3-tier-vpc / 상태 검사 경로 : /wordpress / 고급 상태 검사 설정 : 성공 코드 301
     + 인스턴스 - 이미지 및 템플릿 - 이미지 생성 - demo-3-tier-wordpress
     + 시작 템플릿 : demo-3-tier-template - 이미지는 생성한 이미지 적용 / 인스턴스 유형 : t2.mirco / 키 페어 지정하지 않음 / 네트워크 설정 포함하지 않음
     + Auto Scaling 그룹 : demo-3-tier-wordpress-asg / VPC : demo-3-tier-vpc, 가용 영역 모두 설정
     + 기존 로드 밸런서 여결 - 대상 그룹 설정 후, ELB 상태 확인 켜기
     + 원하는 용량 : 2개, 최소 용량 : 0, 원하는 최대 용량 : 2개
     + 태그 : Name / Demo-3-Tier-Worpress-Asg
     + 로드밸런서 생성 : demo-3-tier-wordpress / VPC : demo-3-tier-vpc / 가용 영역 : Public Subnet / 대상 그룹 : demo-3-tier-target-group
     + 로드밸런서 활성화가 되면, DNS/wordpress로 접속

   - CORS / 보안그룹으로 인해 최초 설치 경로를 바라보는 파일 로딩 불가능 : 직접 로그인해서 WordPress 주소 및 경로 수정 필요
     + DNS/wordpress/wp-admin으로 접속 - 로그인 - Setting - WordPress Address / Site Address 주소를 로드 밸런서 주소로 변경
   - EC2 인스턴스로 직접 들어올 수 없도록 보안 그룹 설정
     + 보안 그룹 생성 - demo-alb-sg / VPC : demo-3-tier / HTTP 유형 / 모든 사용자 (생성 후 이름 지정)
     + 보안 그룹 생성 - demo-ec2-alb / VPC : demo-3-tier / 인바운드 규칙 추가 : 모든 트래픽 / 대상 : demo-alb-sg, SSH / 모든 변경
     + 보안 - 보안 그룹 변경 - 보안 그룹 추가 : demo-ec2-alb (default 제거)
     + 시작 템플릿 : 기존 템플릿에서 새로운 버전 생성 / 보안 그룹 : demo-ec2-alb 
     + Auto Scaling 그룹 - 작업 - 편집 - 시작 템플릿 버전 Latest 선택
     + 로드밸런서 보안 그룹 변경 - 기존 삭제, demo-alb-sg로 변경

   - 모든 검증이 끝나면 리소스 삭제 : AutoScaling 그룹 삭제 / 로드 밸런서 삭제 / RDS 삭제 / S3 버킷 삭제 / 생성한 EC2 삭제 / EFS 삭제 / AMI 및 스냅샷 삭제 
