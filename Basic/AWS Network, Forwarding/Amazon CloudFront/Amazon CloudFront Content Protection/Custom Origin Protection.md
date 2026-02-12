-----
### Cusstom Origin 보호
-----
1. 방법 1 : Custom Header 활용 (CloudFront에서 Header 생성 : Origin에서 해당 Header가 없으면 거부)
<div align="center">
<img src="https://github.com/user-attachments/assets/6ab39fe7-64a9-4ee0-a8eb-b78cc37deca5" />
</div>

2. 방법 2 : Origin에서 CloudFront IP를 제외한 모든 트래픽을 차단
<div align="center">
<img src="https://github.com/user-attachments/assets/675406bc-ccde-4f27-a3b6-1ce721246e81" />
</div>

-----
### Demo
-----
1. EC2 생성 : demo-my-origin-webserver / 키 페어 없이 진행 / 보안 그룹 : default / 사용자 데이터
```
#!/bin/bash
sudo -s
dnf install httpd -y
service httpd start
chkconfig httpd on
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
echo "$INSTANCE_ID" >> /var/www/html/index.html
```
   - 보안 그룹 생성 : demo-allow-only-cf / 인바운드 규칙 : 유형(모든 트래픽), 소스(접두사 목록 : global.cloudfront.origin-facing 선택 (CloudFront에서 사용하는 IP 주소가 자동으로 포함)

   - 인스턴스 - 작업 - 보안 - 보안그룹 변경 - 기존 것 제거 후 demo-allow-only-cf 추가

2. CloudFront Distribution 생성 
3. 보안그룹 설정을 통한 Origin 보호
   - demo-ec2-origin / Origin Type : Other / Origin : EC2 Public DNS 입력 / 프로토콜 : HTTP만 / 보안 보호 비활성화

4. EC2 정리 및 CloudFront Distribution 비활성화
