-----
### Demo - Amazon Route 53 Routing Policy
-----
1. EC2 인스턴스 2개 프로비전
   - demo-route53-seoul
   - 키 페어 없이 계속 진행
   - 보안 그룹 : default
   - 고급 세부 정보 : 사용자 데이터
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
   - 인스턴스 개수 2개 (위 생성 : 1 / 위 생성 : 2)

2. Route 53 레코드 생성
   - 호스팅 영역 - 레코드 생성 - 라우팅 정책 : 단순 라우팅
   - 레코드 이름 : test / 레코드 유형 : A / 값 : 생성된 EC2 Public IPv4 입력 / TTL 0초

3. DNS에서 어떻게 보는지 확인 (Simple Routing Policy)
   - CloudShell 명령어 입력
```
# Cloudshell DNS Lookup 설치
sudo yum install bind-utils net-tools -y
```
```
ns lookup 호스팅DNS주소
```
   - 두 개의 Addrss (EC2 인스턴스)
   - 2번 접속 후 중단
```
sudo -s
service httpd stop
```
   - 새로 고침을 하면 1번 EC2 인스턴스 사용 (크롬 브라우저의 경우)

4. DNS에서 어떻게 보는지 확인 (Weighted Routing Policy)
   - 기존 레코드 삭제 후, 레코드 생성
   - 레코드 이름 : test / 레코드 유형 : A / 값 : 생성된 1번쨰 EC2 Public IPv4 입력 / TTL 0초
   - 라우팅 정책 : 가중치 기반 / 가중치 : 100 / 레코드 ID : my-weighted-routing-01
   - 2번째도 동일
     + 레코드 이름 : test / 레코드 유형 : A / 값 : 생성된 2번째 EC2 Public IPv4 입력 / TTL 0초
     + 라우팅 정책 : 가중치 기반 / 가중치 : 5 / 레코드 ID : my-weighted-routing-02

   - 크롬 브라우저에서는 캐싱되어 있으므로 가중치 100만 계속 출력 : 확인 방법
```
# 크롬 DNS Cache 설정
chrome://net-internals/#dns
```
   - Clear Host Cache 후 확인

5. DNS에서 어떻게 보는지 확인 (Simple Routing Policy)
   - CloudShell 명령어 입력
```
ns lookup 호스팅DNS주소
```

6. DNS에서 어떻게 보는지 확인 (Failover Routing Policy)
   - Route 53 - 상태 검사 생성 - 이름 : demo-hc-1 / 엔드포인트 모니터링 - IP 주소 / EC2 1번재 인스턴스 IP / 경로 : index.html
     + 고급 구성 : 요청 간격 빠름 / 실패 임계값 : 1초
   - Route 53 - 상태 검사 생성 - 이름 : demo-hc-2 / 엔드포인트 모니터링 - IP 주소 / EC2 2번재 인스턴스 IP / 경로 : index.html
     + 고급 구성 : 요청 간격 빠름 / 실패 임계값 : 1초
   - 기존 레코드 삭제 후 생성
     + 레코드 이름 : test / 레코드 유형 : A / 값 : 생성된 1번쨰 EC2 Public IPv4 입력 / TTL 0초
     + 라우팅 정책 : 장애 조치 / 장애 조치 레코드 유형 : 기본 / 상태 확인 : hc-1 / 레코드 ID : demo-failover-01
     + 레코드 이름 : test / 레코드 유형 : A / 값 : 생성된 2번쨰 EC2 Public IPv4 입력 / TTL 0초
     + 라우팅 정책 : 장애 조치 / 장애 조치 레코드 유형 : 보조 / 상태 확인 : hc-2 / 레코드 ID : demo-failover-02

   - 기존 기본이 먼저 보임
   - 기본을 Failover : 기본 EC2 접속 후 다음 명령어 입력
```
sudo -s
service httpd stop
```
   - 상태 검사 : 비정상 확인 후  Clear Host Cache 후 확인
```
# 크롬 DNS Cache 설정
chrome://net-internals/#dns
```

7. DNS에서 어떻게 보는지 확인 (Geolocation Routing Policy)
   - 위치 선택 가능 (기본값 : 지정된 곳 이외)
   - 위치 : 기본값 설정된 레코드 하나 생성
     + 레코드 이름 : test2 / 레코드 유형 : A / 값 : 생성된 1번쨰 EC2 Public IPv4 입력 / TTL 0초
     + 라우팅 정책 : 지리적 위치 / 위치 : 기본값 / 레코드 ID : demo-my-geo-01

   - 위치 : 해당되는 곳만 설정된 곳에만 적용되는 레코드 하나 생성
     + 레코드 이름 : test2 / 레코드 유형 : A / 값 : 생성된 2번쨰 EC2 Public IPv4 입력 / TTL 0초
     + 라우팅 정책 : 지리적 위치 / 위치 : 아시아 / 레코드 ID : demo-my-geo-01

   - test2는 서울 리전에 위치하므로 아시아 위치에 선택된 레코드로 전달

8. DNS에서 어떻게 보는지 확인 (Geoproximity Routing Policy)
   - 라우팅 정책 : 지리 근접성 / 엔드포인트 위치 유형 : AWS 리전 / 위치 : 아시아 태평양
   - 바이어스는 트래픽 정책에서 확인

9. 리소스 정리 : EC2 제거 / 상태 검사 제거 / 레코드는 선택
