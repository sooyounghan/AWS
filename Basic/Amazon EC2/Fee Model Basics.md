-----
### Amazon EC2 요금 모델 기초
-----
1. AWS 가격 정책
   - 미리 내지 않고, 사용한 만큼 내고 : 미리 예치금 등이 없고, 사용한 만큼(On-Demand)만 요금 지불
   - 많이 쓸수록 적게 내고 : 사용하면 사용할수록 단위 가격이 더 저렴해짐
   - 예약할수록 더 적게 낸다 : 미리 요금을 낼 필요는 없지만, 미리 예약하면 더 많이 저렴함
     + 즉, On-Demand는 유용하지만 비싸다 : 약정을 통해 할인 혜택 적용 가능

2. EC2의 요금 구성
   - 인스턴스 요금
   - 데이터 전송
   - Public IPv4 IP
   - 연동 서비스 : ELB, CloudWatch, EBS 등

3. EC2 요금 모델
   - On-Demand : 사용한 시간 만큼 요금 지불
   - Spot Instances : 남는 인스턴스를 저렴하게 사용
   - Reserved Instances : 인스턴스 사용 기간 약정
   - Dedicated : 물리적인 전용 인스턴스 임대 (Dedicated Instance / Dedicated Host)
   - Savings Plan : AWS 컴퓨팅 사용량 약정

4. On-Demand
   - 실행하는 인스턴스에 따라 초당 혹은 시간당 컴퓨팅 파워로 측정된 가격 지불
   - 약정 필요 없음
   - 수요 예측이 힘들거나 유연하게 EC2를 사용하고 싶을 때 가능
   - 한 번 써보고 싶을 때 가능

5. EC2 예약 인스턴스 (Reserved Instances)
   - 예약 인스턴스
     + EC2 인스턴스를 일정 기간 약정하여 요금을 할인 받는 방식
       * On-Demand EC2 사용 요금을 할인 받는 방식으로 적용
       * 할인 받고 싶은 EC2 인스턴스와 같은 Region, 유형 구매 필요
     + 약정 기간이 길수록 더 큰 할인율 적용 : 1년 혹은 3년 선택 가능
     + 최대 72% 저렴

6. Savings Plan
   - 컴퓨팅 파워의 사용량을 약정하는 요금 모델
   - 종류
     + Compute Savings Plans : 다른 서비스(Lambda, Fargate 등과 같이 사용)와 같이 약정
     + EC2 Instance Savings Plans : EC2 인스턴스 패밀리를 지정해서 약정
   - 최대 72% 저렴

7. 스팟 인스턴스 (Spot Instance)
   - AWS에서 보유중인 남는 인스턴스를 저렴한 가격으로 제공 : 가용 영역 별, 인스턴스 유형 별 다른 풀로 관리
   - 최대 90%까지 절약 가능 : 가격은 상황에 따라 변동, 단, 항상 가격은 On-Demand 이하를 보장
   - 단, 인스턴스가 언제 종료될지 예측 불가능 (Spot Instance Interruption)

8. 전용 인스턴스 (Dedicated Instance)
   - 물리적으로 인스턴스 / 호스트 단위로 격리된 서버에서 EC2를 실행
   - 주로 라이선스 이슈 / 퍼포먼스 이슈를 해결하기 위해 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/e6daa6b0-f5d3-4a17-9c57-d995ca15c04c" />
</div>
