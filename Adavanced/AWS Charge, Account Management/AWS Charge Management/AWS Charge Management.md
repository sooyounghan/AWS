-----
### AWS 비용 원칙
-----
1. AWS에서는 사용 전 비용을 지불하지 않음 (일부 예약하는 경우 제외, 예약하면 저렴)
2. AWS에서는 서비스를 사용한 만큼만 비용 지불 : 일정 기간 단위로 약정 (X)
3. AWS에서는 많이 사용한 만큼 적게 냄
   - 예) S3 저장 비용 (GB 당)
     + 처음 50TB / 월 : $0.025
     + 다음 450TB / 월 : $0.024
     + 다음 500TB / 월 초과 : $0.023

-----
### AWS 비용 발생
-----
: AWS는 연산 / 저장 / 데이터 통신에서 비용이 발생
   - 연산 : EC2, RDS 등 서비스를 사용한만큼 발생
   - 저장 : S3, EBS 등 데이터를 저장한 만큼 시간 당 비용 발생
   - 데이터 통신 : 💡 AWS 외부로 나간 데이터에 대해서만 요금 발생

-----
### AWS 내부의 데이터 통신의 비용 
-----
1. 같은 리전 : VPC Internet Gateway를 통한 AWS 서비스 연동은 필요 없음
<div align="center">
<img src="https://github.com/user-attachments/assets/010d67c8-7e82-40b1-8df6-2541c6172fd5" />
</div>

   - 단, NAT Gateway를 활용 시, 데이터 비용 + 시간 당 비용 발생
<div align="center">
<img src="https://github.com/user-attachments/assets/5e0072fc-54ed-4f9b-9e48-d8aed1c2105b" />
</div>

2. VPC Endpoint
<div align="center">
<img src="https://github.com/user-attachments/assets/e83639ce-bc1f-4614-a234-98c5f232f235" />
</div>

   - VPC 내부에서 같은 AZ 간 통신은 비용 없음
   - VPC 내부에서 다른 AZ 간 통신은 비용 발생 : 서비스 자체적으로 비용이 발생하지 않는 경우도 존재 (예) S3, RDS 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/6c7bfb5b-8435-4d30-97dc-29f2991055e2" />
<img src="https://github.com/user-attachments/assets/8ded9c6c-5500-4a1c-a99f-14f064f5f5a8" />
</div>

3. 다른 리전 : 리전 간 통신 비용 발생
<div align="center">
<img src="https://github.com/user-attachments/assets/3253ef7f-adfb-4002-a006-268629479cd8" />
</div>

4. 기타 - On-Premise 통신 : 시간 당 비용 + 나간 데이터에 대해 비용 발생
<div align="center">
<img src="https://github.com/user-attachments/assets/f32b48d7-ab6f-4a07-8a7e-9490947134f7" />
</div>

-----
### 총 소유 비용 (TCO, Total Cost of Ownership)
-----
1. 인프라 환경을 운영하는 경우 발생하는 총 소유 (자산 매입 + 운용) 비용
2. 단순히 서버 / 인프라 요금 비교가 아닌 관리 비용, 효율성, 안정성, 확장성, 연계 가능성 등 모든 비용 비교
   - 예) 데이터 센터 운영의 경우
     + 인건비(24 / 7), 전기료, 건물 관리비, UPS, 냉각 시스템, 청소비
     + 보안 유지비 (출입 통제, CCTV, 네트워크 보안 장비 등)
     + 노후화에 따른 장비 성능 저하, 추후 신규 시스템 업그레이드 비용
     + 확장성, 사용 종료에 따른 비용

-----
### Capex VS Opex
-----
1. Capex (Capital Expenditure)
   - 시설 혹은 자원을 사용하기 위해 미리 지불하는 비용
   - 예) 서버 컴퓨터 비용, 서버실 설치 등
   - AWS의 경우 Capex가 거의 없음

2. Opex (Operational Expenditure)
   - 사용에 따라 발생하는 비용
   - 예) 전기비, 인건비 등
   - AWS를 사용하여 Opex를 획기적으로 줄일 수 있음

-----
### 주요 비용 관련 서비스
-----
1. AWS Budgets : 예산 범위를 지정하고 이를 넘어서거나 넘어설 것으로 예상되는 경우 조치
2. AWS Cost Explorer : AWS 서비스의 비용 및 사용량을 분석하는 서비스
<div align="center">
<img src="https://github.com/user-attachments/assets/a70e83fd-c9b8-4b27-9211-30145b556eb6" />
<img src="https://github.com/user-attachments/assets/fa45c06b-a6a1-456e-9f84-9ede9ebf0d49" />
</div>

3. AWS Pricing Calculator : AWS의 비용을 미리 추정해볼 수 있는 서비스
   - 이미 발생한 비용이 아닌 미래의 비용을 추정
   - 리전별, 서비스별 비용 추정 가능
   - 추정된 비용의 저장, 공유, 내보내기 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/3a9b1961-1b90-4543-8e90-f81cdb94ec87" />
</div>
