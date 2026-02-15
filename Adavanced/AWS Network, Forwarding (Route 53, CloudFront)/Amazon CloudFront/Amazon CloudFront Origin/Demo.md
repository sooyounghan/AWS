-----
### Demo
-----
1. Region : 서울
2. EC2 인스턴스 프로비전
   - demo-cf-origin / 키 페어 없이 계속 진행 / 보안 그룹 : default
   - 사용자 데이터
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

3. S3 버킷 생성
   - demo-origin-bucket-{계정 ID}
   - 퍼블릭 액세스 차단 해제
   - flower.jpg 업로드
   - 버킷 권한 - 버킷 정책 편집 
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::[버킷명]/*"
        }
    ]
}
```

4. CloudFront
   - 배포 (글로벌 서비스이므로 리전 선택 불가) - 배포 생성
     + demo-my-cloudfront
     + Single website or app
     + Origin Type : Other (Custom Origin)
       * Origin - Custom Origin : EC2 인스턴스 Public DNS
       * 오리진 설정 - Customize origin settings - 프로토콜 : HTTP만 해당 (포트 80)
       * WAF (방화벽) : 보안 보호 비활성화
     + 배포가 굉장히 오래걸림 (글로벌 서비스)

   - 배포 생성
     + demo-s3-origin
     + Origin Type : Amazon S3
     + S3 Origin : demo-origin-bucket-{계정ID}
     + Grant CloudFront access to origin (비활성화) : CloudFront가 이를 기반으로 S3 버킷에 액세스 권한 부여
     + 보안 보호 비활성화

   - EC2 Origin Domain 복사 후 확인
     + EC2 인스턴스 연결 후 확인
```
sudo -s
cd /var/log/httpd

dir

tail -f access_log
```

   - EC2 Public DNS로 접속 또는 CloudFront로 접속 해도 모두 로그 출력 : CloudFront에서도 EC2 인스턴스를 Origin으로 보고 있음
   - CDN (캐싱) 설정 : demo-my-cloudfront
     + 동작 - 편집 - 캐시 키 및 원본 요청 - Legacy cahce settings - 기본 TTL 10 (10초 동안 캐싱)
     + 수정될 때마다 다시 배포를 하므로 오랜 시간이 소요
   - EC2 접속하면 로그 출력, CloudFront는 캐싱을 하므로 10초 동안 캐싱된 페이지를 보여주므로 출력되지 않음 (10초 이후에는 로그 출력)

5. S3 Origin
   - Domain 복사 후 접속 : Access Denied (해당 DNS에는 명시한 경로를 설정하지 않았기 때문임)
   - /flower.jpg로 접속하면 가능 (현재 캐싱 설정하지 않아 계속 요청)

6. Origin Group을 통한 CloudFront 역할 확인
   - EC2 인스턴스 2개 프로비전 : demo-origin-group / 인스턴스 개수 : 2 / 키 페어 없이 계속 진행 / 보안 그룹 : default / 사용자 데이터
```
#!/bin/bash
sudo -s
sudo yum install -y httpd 
systemctl start httpd
chkconfig httpd on
echo "hello,world!" >> /var/www/html/page.html
echo "ohhh its 404" >> /var/www/html/backup.html
```
   - 기존 demo-my-cloudfront 배포에 Origin Group 생성
     + 원본 - 원본 생성 - 오리진 선택 : EC2의 demo-origin-group 2개 - 프로토콜 : HTTP만 해당
     + CloudFront 원본 3개 (기존 1개 + 2개)
     + 원본 그룹 생성 - 위 2개 선택 / 이름 : demo-origin-gorup / 장애 조치 기준 : 404 찾을 수 없음
     + 동작 - 편집 - 원본 및 그룹 : demo-origin-group / 캐시 키 및 원본 요청 : TTL - 0초 (캐싱을 하지 않은 상태)
       * 2개를 제외한 원본 및 EC2 인스턴스 제거
       * CloudFront 베포 도메인을 통해 접속 (+ /page.html) / EC2 이름 변경 : 1개 - primary, 1개 : secondary)
       * Primiary EC2에 접속하여, 로그 확인 : 새로고침 할 때마다 요청 받음 (캐싱 비활성화)
```
sudo -s
cd /var/log/httpd

dir

tail -f access_log

cd /var/www/html/
rm page.html
```

   - page.html 삭제로, 404에러가 발생해야 하지만, 정상적 출력 : 원본 그룹에 Failover 정책이 추가 (Secondary로 요청됨)
   - Secondary EC2에 접속하여 확인 : 해당 Secondary에서 정상적 출력
```
sudo -s
cd /var/log/httpd

dir

tail -f access_log
```
   - page.html 제거 : 404 오류 발생
```
rm page.html
```
   - Custom Response 생성
     + demo-my-cloudfront - 오류 페이지 - 사용자 정의 오류 응답 생성 - HTTP Error Code : 404 / TTL : 0 / Customize error response: Yes (Response page path : /backup.html, HTTP Response Code : 200)
     + 배포가 될 때까지 대기 후, DNS/page.html로 접속하면, backup.html을 404 에러가 발생할 때 보이도록 설정했으므로, 해당 페이지 출력

7. 리소스 정리 : EC2 정리 / CloudFront - 배포 정리 (비활성화 후 삭제) 
