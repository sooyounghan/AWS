-----
### CloudFront의 파일 관리
-----
1. 싱글 파일 : 파일명을 유지한 채로 캐시 만료 처리, 업데이트 등 처리
   - 별도로 클라이언트 업데이트 필요 없음
   - 캐시 만료 전 제공 파일을 업데이트 하려면 Invalidation 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/75e25b3c-7c7c-4614-b7b2-ea7f2ed88665" />
</div>

2. 버저닝 : 파일 이름에 다양한 방법으로 버전을 두어서 관리
   - 별도로 Invalidation 필요 없음
   - 파일 업데이트 시 클라이언트 업데이트 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/cbb929e6-5240-4af2-af72-10e84953b8eb" />
<img src="https://github.com/user-attachments/assets/b01ea95a-f20e-46c4-b12e-f52609c7bae6" />
<img src="https://github.com/user-attachments/assets/a5d87e22-c2be-4dec-bff1-597eb180135f" />
</div>

-----
### Invalidation
-----
1. 캐시 만료 전 파일을 갱신
2. 버저닝이 아닌 형태로 파일 제공할 경우, 캐시 만료 전 새 파일을 제공하고 싶다면, Invalidation 필요
3. 경로 기반
    - 예시) ```/img/img1.png, /img/*, /img/img*```
4. 한 번에 최대 3000파일까지 Invalidate 가능 (예) 100개씩 30 Invalidations 또는 1000개씩 3 Validations)
5. 한 달에 1000 Path Invalidations은 무료 (계정 전체 Distribution 통합), 이후 한 번 경로당 $0.005

-----
### Demo
-----
1. EC2 인스턴스 : demo-cf-instance / 키 페어 없이 사용 / 보안 그룹 : default / 유저 데이터
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

2. CloudFront - 배포 : demo-invalidation / 타입 : Other / Origin : EC2 Public IPv4 DNS / HTTP만 해당 / 보안 보호 비활성화

3. 동작 - 편집 - 캐시 키 및 원본 요청 - Legacy Cahce Settings - 기본 TTL : 100
4. 배포 도메인 네임 복사 후 접속
5. EC2 인스턴스 접속 - 명령어 입력
```
sudo -s
cd /var/www/html
dir
nano index.html
```
   - 인스턴스 ID 확인, 이후 Hello, World 후 웹 재접속 : 100초 이후 변경되므로 변경되지 않음
   - 따라서, CloudFront - 배포 - 해당 배포 - 무효화 - 무효화 생성 - 객체 경로 : /index.html
   - 다시 한 번 재접속하면, 변경된 내용으로 출력

6. 리소스 정리 : EC2 종료 / CloudFront 배포 종료
