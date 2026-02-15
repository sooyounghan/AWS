-----
### Demo - Route 53과 도메인 등록과 활용
-----
1. Part 1
   - Route 53에서 도메인을 등록해서 Route 53에서 관리하도록 설정
     + Route 53은 글로벌 리전에서만 가능
     + 등록된 도메인 - 도메인 등록 - 검색 결과에서 사용할 도메인 선택 후 결제 진행 (자동 갱신 : 1년마다 활성)
     + 개인정보 입력 (도메인 등록 시 한 번만 사용하면 됨)
     + 등록 상태에 대한 이메일 수신 후 인증하면, 등록된 도메인에 사용 가능한 도메인 확인 가능
     + 등록된 도메인이 호스팅 영역에 자동으로 생성

   - 이후 다양한 레코드를 생성해서 AWS 리소스와 연결 (EC2, S3, ALB)
     + EC2 - demo-ec2-web / 키 페어 없이 계속 진행 / 보안 그룹 : default - 사용자 데이터
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

   - Public IP로 접속 확인 및 Pubic DNS로 연결 확인
   - Route 53 - 호스팅 영역 - 생성한 호스팅 접속 - 레코드 - 레코드 생성 - 레코드 이름 : web / 값에 EC2 Public IP 값 입력 / TTL은 1분 설정 / 레코드 유형 : IPv4 (A) - 레코드 생성
     + ```web.주소```로 연결 확인 : 원하는 도메인으로 호스팅 가능
     + IP 주소 대신 Public IPv4 DNS 사용 : 레코드 생성 - 레코드 유형을 CNAME 선택 후, 값에 Public IPv4 DNS 입력 / 레코드 이름은 webdns / TTL 1분
     + ```webdns.주소```로 연결 확인

   - S3 버킷 생성 : demo-my-static-web-{계정ID} / 퍼블릭 액세스 - 버킷 만들기
     + 권한 : 버킷 정책 - 편집 후 저장 / 속성 - 정적 웹 사이트 호스팅 : 인덱스 문서 : index.html 저장 / index.html를 객체로 업로드 이후 정적 웹 사이트 호스팅 도메인으로 접속
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

   - S3 정적 웹 사이트 호스팅과 Route 53 연결 : Route 53 - 도메인 접속 - 레코드 생성 - 레코드 이름 : static / 레코드 유형 : CNAME / 값 : S3 위 정적 웹 사이트 호스팅 주소(http:// 삭제) / TTL 1분 
     + 💡 404 Not Found 발생 : Route 53 레코드 명과 S3 버킷명이 동일해야 함 (따라서, 현재 사용 불가)
     + 버킷 생성 - ```static.웹사이트.com``` - 퍼블릿 액세스 차단 - 버킷 생성 후, 동일하게 실시
     + 오류가 발생했던 레코드 선택 후 레코드 편집 - 값에 이름을 동일하게 설정한 버킷의 정적 웹 사이트 호스팅 값 입력하면, 정상적 출력
     + 💡 루트 도메인의 경우 레코드 이름 생략 가능하지만, 생성 불가능 : APEX 도메인은 CNAME 사용 불가
       * 해결 방법 : Alias 별칭 (레코드 유형 - A / 별칭 활성화 / 트래픽 라우팅 대상 - 엔드포인트 선택 : S3 웹 사이트 엔드포인트에 대한 별칭 / 리전 - 아시아 태평양(서울) / 자동으로 S3 버킷 주소 선택 후 생성 

   - ALB를 Route 53와 연결
     + EC2 - 대상 그룹 : demo-route53 / 상태 검사 : /index.html / 생성한 EC2 선택 - 대상 그룹 생성
     + ALB 생성 : demo-route53-test / 네트워크 매핑 가용 영역 모두 선택 / 보안 그룹 : default / 리스너 및 라우팅 : 기본 작업 - demo-route53 - 로드 밸런서 생성
     + ALB DNS 생성 후 입력
     + 레코드 생성 - alb / 레코드 유형 : A / 별칭 활성화 / 트래픽 라우팅 대상 : ALB에 대한 별칭 / 리전 : 서울 / 자동으로 생성된 ALB 선택 후 호스팅 후 입력 (하지만, HTTPS는 미지원)

   - 리소스 정리 : 로드 밸런서 정리 (작업 - 로드 밸런서 삭제) / EC2 삭제

2. Part 2
   - 외부에서 도메인 구입 후 Route 53에서 생성한 Hosting Zone의 NS 서버로 변경 : kr 등 Route53에서 구매할 수 없는 경우 / 이미 도메인을 보유했을 경우 
   - 예) 카페 24에서 도메인 구매 (예) ```awsclass.kr```)
     + 원하는 도메인 검색 후 구매
     + 도메인 확인 : 네임 서버 변경 필요 (```awsclass.kr```에 대한 네임 서버들을 Route 53에서 제공하는 NS 서버로 변경)
     + 수동으로 호스팅 영역 생성 필요
     + 호스팅 영역 생성 - ```awsclass.kr``` - 퍼블릭 호스팅 영역 : 기본적으로 NS 레코드와 SOA 레코드 생성
       * NS 서버에 해당하는 값들을 카페 24의 NS 서버로 변경
       * 보통 다른 서버 - 1차 / 2차 / 3차 / 4차 네임 서버 입력 필요 (상황에 따라 .은 호환되지 않으므로 제외 후, IP 확인) - 이후 변경

   - S3 생성 - ```awsclass.kr``` - 퍼블릭 액세스 허용 - 생성
     + 권한 - 버킷 정책 - 편집 / 속성 - 정적 웹 사이트 호스팅 활성화 (인덱스 문서 : index.html)
     + index.html 업로드
     + ```awsclass.kr``` 레코드 생성 - 레코드 이름 생략 / 레코드 유형 : A / 별칭 활성화 / S3 웹 사이트 엔드포인트에 대한 별칭 / 서울 리전 / S3 버킷 선택 후 레코드 생성
     + ```awsclass.kr```로 연결 확인

3. 💡 둘 다 비용 발생
   - 도메인 구입 비용 발생 (Route 53에서 등록하더라도 크레딧 사용 불가능)
   - Route 53은 프리티어가 없으므로 매달 Hosted Zone 당 $0.5 불 + Query 비용 발생
   
