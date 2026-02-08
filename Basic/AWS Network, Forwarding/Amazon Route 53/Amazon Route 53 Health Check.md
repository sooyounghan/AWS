-----
### Amazon Route 53 Health Check
-----
1. Route 53에 설정한 리소스 (웹 애플리케이션, 서버 등)의 상태를 모니터링 하는 기능 : Routing Policy에서 활용 / Amazon CloudWatch 경보 설정하여 알림 처리 가능
2. Latency 정보 확인 가능
   - TCP Connection 연결 수립까지 걸린 시간
   - HTTP / HTTPS First Byte를 받기까지 걸린 시간 : SSL / TLS Handshake까지 걸린 시간
3. CloudWatch Metric으로 Health Check 상태 기록 / 알람 가능 : US-East-1 Region 전용
4. 모니터링 대상
   - 특정 리소스
   - 다른 Health Check
   - Amazon CloudWatch 경보
   - Route 53 Application Reovery Controller

5. 모니터링 대상 - 리소스
   - IP 주소, 도메인에 주기적으로 요청을 보내 응답 여부를 확인하여 가용성을 확인
   - 상태 확인 방법
     + 전 세계의 다수의 Health Checker에서 정해진 프로토콜 / 주기를 요청을 보내서 상태 확인
     + 주기 : 10초 (추가요금) 또는 30초 (Checker 별 Sync 없음)

   - 두 가지 지표를 기준으로 상태 판단
     + 응답 속도 (HTTP(연결 수립) : 4초 / TCP : 10초 / HTTP String Match : 2초 안에 값 확인)
     + 응답 내용 및 지정한 실패 횟수를 연속으로 넘었는지 여부

   - 해당 기준으로 전체 Health Checker의 18% 초과가 Healthy 상태를 유지해야 Health 상태로 판단
   - 주의 : AWS 바깥의 엔드포인트를 대상으로 할 경우 추가 요금 발생
<div align="center">
<img src="https://github.com/user-attachments/assets/c1695925-0464-4cff-85e5-8a37b4541ec2" />
</div>

6. 모니터링 대상 - 리소스 지정 가능 값
   - 모드
     + IP 주소 : IPv4, IPv6 (로컬 / Private / Multicast 등은 체크 불가능)
       * HTTP / HTTPS → Status 2XX, 3XX
       * TCP
     + 도메인
       * HTTP / HTTPS → Status 2XX, 3XX
       * IPv4만 지원, 즉, A Record 외에는 Fail 처리

   - 매칭 문자열 : 응답의 Body에 특정 문자열이 있는지 확인
   - Health Check Region : 최소 3개
   - 기타 : 포트 / 경로 / 주기 / SNI 지원 / Latency Graphs / Inverse Check 등

7. 모니터링 대상 - 다른 Health Check
   - 다른 Health Check을 모니터 (예) 3개 이상의 설정한 Hatcheck의 상태 검사가 실패할 경우 경보 / FailOver 등)
   - 상태를 정하기 위한 Health Check 숫자 지정 가능
     + 모든 Health Check 성공이면, 성공
     + 단, 하나의 Health Check 성공이면, 성공
     + 지정한 Health Check 숫자 중 N개 이상이 성공이면, 성공
<div align="center">
<img src="https://github.com/user-attachments/assets/0b03af3c-6e32-4a04-aa67-89da56357987" />
</div>

8. 모니터링 대상 - CloudWatch 경보
   - CloudWatch 경보 상태를 모니터링
   - 주의
     + CloudWatch 경보의 경보 상태가 아닌 직접 데이터를 모니터링 : 즉, CloudWatch 경보보다 조금 더 민감하게 반응
     + Standard Resolution (60초마다 수집) 경보만 모니터링 가능
     + Average, Minimum, Sum, SampleCount만 모니터링 가능
     + Math Metric 사용 불가능
     + CloudWatch 경보가 변경되었을 경우, Route 53 Health Check 수동으로 업데이트 필요

   - 데이터가 충분하지 않을 경우 (Insufficient Data 상태) 상태 지정 가능 (예) Insufficient Data일 때, Healthy, Unhealthy, Last Known Status)

9. Demo - 리소스 Health Check 모니터링
   - EC2 인스턴스를 프로비전하고, Route 53 Health Check 구성 : EC2를 두 개 만들어 Health Check를 구성하고, 이 두 Health Check를 모아서 모니터링하는 Check 생성
   - Health Check Fail 시, SNS를 통해 이메일 받아보기 
