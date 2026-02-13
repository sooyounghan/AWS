-----
### ELB LCU Reservation
-----
1. LCU (Load Balancer Capacity Units) : ALB가 트래픽을 처리하는 단위
   - 1 LCU
     + 25개의 새로운 커넥션 / 초
     + 3,000 / 분 연결 숫자 또는 1500 / 분 TLS
     + 1GB / 시간의 EC2 컨테이너, IP 주소 또는 0.4 GB / 시간의 Lambda 함수 트래픽
     + 1,000 룰 평가 / 초 (10개 무료)

   - 이 중 가장 높게 측정된 것을 기준으로 계산

2. LCU Reservation
   - ALB : 트래픽, Bandwith, 동시 연결 숫자 등 + WAF 혹은 Lambda 등의 처리를 위한 연산에 따라 스케일링 (일반적으로 5분에 2배 증가 가능(예) 5Gbps 사용 시 5분 후 10Gbps까지 증설))
   - NLB : Bandwith 기준으로만 스케일링 (1분에 3Gbps 증가)
   - LCU Reservation : 증설 속도를 따라오지 못할 정도로 순간적으로 많은 트래픽이 예상되는 경우 미리 최저 리소스를 맞추는 기능 (예) 이벤트 오픈 당일 / Black Friday 행사 등)
   - LCU 확인은 콘솔에서 Amazon CloudWatch로 확인 가능 (ALB은 PeakLCU, NLB은 ProcessedBytes를 기준으로 연산을 통해 확인)

3. Sticky Session
<div align="center">
<img src="https://github.com/user-attachments/assets/d6360173-7afe-4780-ab93-9664980189b5" />
<img src="https://github.com/user-attachments/assets/3f11c919-fce1-4957-899e-0dd0fe8a85cd" />
</div>

4. ALB Stickiness
   - Stickiness : ALB에서 한 번 연결한 대상에 지속적으로 연결할 수 있도록 지원하는 서비스
   - Sticky Session : 애플리케이션이 EC2 인스턴스와 지속적 연결 보장
     + 쿠키를 발급하여 해당 쿠키의 내용을 기반으로 대상 선택
     + 두 가지 방식 : ALB가 발급 / 애플리케이션에서 직접 발급
     + 최소 1초에서 최대 7일 기간 한정
<div align="center">
<img src="https://github.com/user-attachments/assets/359c1994-5601-4174-a19d-6e228060a46e" />
<img src="https://github.com/user-attachments/assets/5d275b4d-ecca-42cf-a0f6-d4aca73f1887" />
<img src="https://github.com/user-attachments/assets/bdc8c7dc-a7c3-486d-bbe3-a88ecb863819" />
</div>

   - Target Group Stickiness : ALB 뒤 여러 대상이 있을 경우 특정 대상 그룹(Target Group)으로 연결 보장 (예) 배포 상황 등)
     + 설정을 통해 일정 시간 동안 특정 요청은 특정 대상 그룹으로만 전달
     + 설정한 시간이 만료되면 기본 로직 적용 (기본 로직 : 가중치 기반 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/57aae038-b2d5-42ca-a723-d08aefd5624c" />
</div>
