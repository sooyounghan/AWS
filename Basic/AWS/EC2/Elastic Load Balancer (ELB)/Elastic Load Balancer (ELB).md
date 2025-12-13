-----
### Elastic Load Balancer (ELB)
-----
1. 둘 이상 가용 영역에서 EC2 인스턴스, 컨테이너, IP 주소 등 여러 대상에 걸쳐 수신되는 트래픽을 자동으로 분산하며, 등록된 대상의 상태를 모니터링하면서 상태가 양호한 대상으로만 트래픽을 라우팅
<div align="center">
<img src="https://github.com/user-attachments/assets/5db0e0ac-1b85-48a1-ab15-a2b5088a33ff" />
<img src="https://github.com/user-attachments/assets/83a977fe-20e4-4b3d-9204-286a86d34d88" />
<img src="https://github.com/user-attachments/assets/ee656f5f-e655-4c2b-aad9-d08cc3c2a0c6" />
<img src="https://github.com/user-attachments/assets/c1d85a14-4bcc-48bf-871a-54d1fe5200d4" />
<img src="https://github.com/user-attachments/assets/8056abc4-125a-47bd-8115-7d52e74304ee" />
</div>

2. 즉, 다수의 EC2에 트래픽을 분산시켜주는 서비스
3. 총 4가지 종류
   - Applicaation Load Balancer : OSI Model Layer 7 / 트래픽을 모니터링하여 라우팅 가능 (예) ```image.sample.com``` : 이미지 서버, ```web.sample.com``` : 웹 서버로 트래픽 분산)
   - Network Load Balancer : OSI Model Layer 4 / 빠르며, TCP, UDP 기반 빠른 트래픽 분산 / Elastic IP 할당 가능 (IP 고정 가능)
   - Classic Load Balancer : 예전에 사용되었던 타입으로, 현재는 잘 사용하지 않음
   - Gateway Load Balancer : OSI Model Layer 3 / 트래픽을 먼저 확인하며, 가상 어플라이언스 배포 및 확장 관리를 위한 서비스

4. Health Check : 직접 트래픽을 발생시켜 인스턴스가 살아있는지 확인
5. EC2 Auto-Scaling과 연동 가능
   - ELB + Auto-Scaling
     + Auto-Scaling을 통해 EC2 인스턴스의 숫자를 관리하고, ELB를 통해 분산 트래픽 처리
     + Auto-Scaling의 인스턴스 증감과 같이 ELB에 연결
<div align="center">
<img src="https://github.com/user-attachments/assets/933ab2d4-6223-4080-9f56-498b0f4c1ce7" />
</div>

6. 지속적으로 IP 주소가 바뀌며, IP 고정 불가능 : 항상 도메인 기반으로 사용
7. 대상 그룹 (Target Group)
   - ELB가 라우팅 할 대상 집합
   - 구성
     + 대상 종류 : Instance, IP, Lambda, ALB
     + 프로토콜 : HTTP, HTTPS, gRPC, TCP 등
     + 기타 설정 : 트래픽 분산 알고리즘, 고정 세션 등
<div align="center">
<img src="https://github.com/user-attachments/assets/12c2550b-0f11-42cd-afb0-97723ff3bdb4" />
</div>

8. 리스너 (Listener)
   - ALB로 들어오는 요청을 처리하는 주체 : 들어오는 트래픽의 프로토콜 + 포트 단위
   - 규칙(Rule)으로 ALB에서 어떤 요청을 받을지, 요청을 어떻게 어디로 처리 할지 결정
     + 예) http : 8080 포트로 트래픽을 받아서 A 대상 그룹의 80 Port 포트로 배분
     + 예) https : https 프로토콜(443) Port로 트래픽을 받아 B 대상 그룹의 80번 Port로 배분
     + 예) HTTP POST 요청이 들어왔을 경우 지정된 응답 전달 (에러 페이지 등)
   - 규칙을 활용해 다양한 조건에 따라 트래픽 배분 가능 : 활용 가능한 조건) Header, QueryString, Source IP, Method 등
   - 들어온 트래픽 처리 방식 : Forward, Redirect, Fixed-Response, Coginto 인증 등
<div align="center">
<img src="https://github.com/user-attachments/assets/417fbed4-f6ef-4f18-b57a-1b79951b4cec" />
</div>

9. ALB 비용
    - Provision 시 시간당 비용 청구
      + 기본 : $0.0225/Hour
      + $0.007/LCU-Hour

     - LCU(Load Balancer Capacity Units) : ALB가 트래픽을 처리하는 단위
       + 1 LCU
         * 25개의 새로운 커넥션/Second
         * 3,000/Min 연결 숫자 또는 1500/Min TLS
         * 1GB/Hour의 EC2, 컨테이너, ip 주소 혹은 0.4GB/Hour의 Lambda 함수 트래픽
         * 1,000 리스너 룰 평가 (10개 무료)

       + 이 중 가장 높게 측정된 것 기준 계산
     - 예시
       + 조건
         * 초당 1개의 새로운 연결 요청, 2분 지속
         * 각각 초당 5 요청/초를 보내며 총 300 KB/sec 응답
         * 리스너 평가 룰 60개
       + 신규 연결 : 1 LCU는 25연결/초, 현재 초당 연결은 1개/초 - 따라서, 1/25 LCU = 0.04 LCU
       + 연결 지속 : 1 LCU는 3000연결/분, 현재 초당 연결은 1연결 X 2분은 120연결, 따라서 120/3000 = 0.04 LCU
       + 트래픽 : 1 LCU는 1GB/시간, 현재 트래픽은 300KB/초 응답 = 1.08 GB/시간, 따라서 1.08/1 = 1.08 LCU
       + 룰 평가 : 1 LCU는 1,000/초 역할 평가, 현재 룰 갯수는 60, 따라서 (60 - 10(프리)) * 5 = 250/1000 = 0.25 LCU
       + 가장 높은 것 : 1.08 LCU
       + 따라서, 비용은 (1.08 LCU * $0.008) + $0.0225 = $0.03114

