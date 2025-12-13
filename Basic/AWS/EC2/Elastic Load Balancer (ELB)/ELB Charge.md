-----
### Elastic Load Balancer 요금
-----
1. ELB 비용
   - 기본적으로 시간당 프로비전 비용
   - 추가적으로 트래픽 / 사용량 비용
  
2. ALB 비용 (서울 Region 기준)
    - Provision 시 시간당 비용 청구
      + 기본 : $0.0225/Hour
      + $0.008/LCU-Hour

     - LCU(Load Balancer Capacity Units) : ALB가 트래픽을 처리하는 단위
       + 1 LCU
         * 25개의 새로운 커넥션/Second
         * 3,000/Min 연결 숫자 또는 1500/Min TLS
         * 1GB/Hour의 EC2, 컨테이너, IP 주소 혹은 0.4GB/Hour의 Lambda 함수 트래픽
         * 1,000 리스너 룰 평가 / 초 (10개 무료)

       + 이 중 가장 높게 측정된 것 기준 계산
         
     - 예시
       + 조건
         * 초당 1개의 새로운 연결 요청, 2분 지속
         * 각각 초당 5 요청 / 초를 보내며 총 300 KB / 초 응답
         * 리스너 평가 룰 60개
<div align="center">
<img src="https://github.com/user-attachments/assets/a2b43780-04cf-43c1-9d5b-7f8a41a22695" />
</div>  
    
   - 가장 높은 것 : 1.08 LCU
   - 따라서, 비용은 (1.08 LCU * $0.008) + $0.0225 = $0.03114

3. NLB 비용
   - 프로비전 시 시간당 비용 청구 (서울 Region 가준)
     + $0.0225/Hour
     + $0.006/NLCU

   - NLCU (Network Load Balancer Capacity Units) : NLB가 트래픽을 처리하는 단위
     + 1 NLCU
       * 800 TCP 연결, 400 UDP / 초
       * 100,000 TCP 연결 유지(1분 단위), 50,000 UDP
       * 1GB / 시간 EC2, 컨테이너, IP, ALB 대상 트래픽

   - 예시
     + 초당 1개의 TCP 연결, 2분 지속
     + 300 KB / 초 응답
<div align="center">
<img src="https://github.com/user-attachments/assets/36ab691c-66cb-4144-bc1f-5a4f12d0465c" />
</div>

   - 가장 높은 값 : 1.08 NLCU
   - 비용 : (1.08 NLCU * $0.006) + $0.0225 = $0.00648

4. 비용 계산기 : ```https://calculator.aws/```
