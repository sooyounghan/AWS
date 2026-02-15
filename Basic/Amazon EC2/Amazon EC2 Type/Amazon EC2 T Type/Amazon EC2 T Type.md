-----
### 인스턴스 유형
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/adeecd5d-bfbf-478a-80cb-2a484156828b" />
</div>

-----
### T 인스턴스
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/97802f3e-3ba1-4103-b7f6-168d11d88519" />
</div>

1. Burstable Performance Instances(버스트 가능한 성능 인스턴스)
   - 다른 인스턴스 타입처럼 고정된 용량의 CPU 자원을 제공하지 않음
   - CPU 크레딧 제도
     + CPU 사용량이 기본 수준(Baseline) 이상(Burst)이라면, 크레딧 차감
     + CPU 사용량이 기본 수준(Baseline) 미만이라면, 크레딧 제공

   - 베이스라인(Baseline) : CPU 사용량을 통해 크레딧의 소모와 증가가 같은 지점 (인스턴스 크기에 따라 다름)

   - 즉, 평소에는 Baseline 밑으로 유지해 크레딧을 모으고, 꼭 필요한 상황에 크레딧을 소모해 Burst
   - 크레딧이 없다면 최악의 경우 CPU 사용량이 5% 미만으로 제한됨

2. CPU 크레딧과 베이스라인
<div align="center">
<img src="https://github.com/user-attachments/assets/6b5be66b-b7a4-4178-a394-2f6c44e91484" />
</div>

   - T 인스턴스는 총 3가지 상태 중 하나
     + CPU 사용 < 베이스라인 : 크레딧 증가
     + CPU 사용 > 베이스라인 : 크레딧 소모
     + CPU 사용 = 베이스라인 : 크레딧 증감 없음
<div align="center">
<img src="https://github.com/user-attachments/assets/d1a014a4-4959-4468-9d77-38ec03411596" />
</div>

   - CPU 크레딧 : vCPU가 사용하는 시간 단위
     + 1 CPU Credit : 1 vCPU * 100% Utilization * 1 minute
     + 1 CPU Credit : 1 vCPU * 50% Utilization * 2 minutes
     + 1 CPU Credit : 2 vCPU * 25% Utilization * 2 minute
   - 시간당 크레딧의 추가 : 각 인스턴스 별로 지정된 베이스라인으로 계산
     + 공식 : vCPU 숫자 x 베이스라인 x 60분
     + 예) T3.nano의 (baseline : 5%) 경우 6크레딧 부여 (2 vCPU * 5% * 60 minutes = 6 Credit)

3. CPU 크레딧의 추가와 소모
   - CPU 크레딧은 CPU 사용량이 베이스라인 미만일 때, 지속적 추가 (MilliSecond 단위)
   - CPU 사용량이 베이스라인 이상일 때, 지속적으로 소모 (MilliSecond 단위)
   - 각 인스턴스 사이즈 별로 최대한 저장 가능한 크레딧의 한계가 존재
     + 일반적으로 시간 당 얻을 수 있는 최대 크레딧 X 24
     + 예) t3.nano의 경우 6 (시간당 얻는 크레딧) * 24 = 144 Credit
     + 이 이상 얻는 크레딧은 버려짐
<div align="center">
<img src="https://github.com/user-attachments/assets/4938a670-1126-46bd-b596-0da7e801e125" />
</div>

4. T 인스턴스 모드
   - 스탠다드 모드(Standard Mode)
     + 베이스라인 이상으로 Burst가 발생할 경우, 미리 저장된 크레딧을 소모해 CPU 사용
     + 크레딧이 없다면, 베이스라인 이상으로 CPU 사용 불가

   - 언리미티드 모드(Unlimited Mode)
     + 제한없이 필요한 만큼 CPU 사용
     + 베이스라인 이상으로 Burst가 발생할 경우, 미리 저장된 크레딧을 소모해 사용
     + 크레딧이 없을 경우, 24시간 안으로 크레딧을 빌려 충당
     + 24시간 안에 베이스라인 미만으로 CPU를 유지해 추가된 크레딧으로 갚기 가능
     + 갚지 못한 크레딧은 비용을 지불해 충당

   - T2는 스탠다드가 기본, T3은 언리미티드가 기본
<div align="center">
<img src="https://github.com/user-attachments/assets/41268063-689f-4dc5-871e-8fddae836b70" />
<img src="https://github.com/user-attachments/assets/27ca8f6b-faea-4fe1-a557-100d74067067" />
</div>

5. Launch Credit
   - T2 인스턴스의 경우 : Launch Credit 제공
     + 인스턴스가 처음 생성될 떄 제공
     + 생성 직후에 Burst로 진입할 경우 대비
     + Standard 모드가 Default

   - T3의 경우 : Unlimited Mode가 Default이므로 Launch Credit이 부여되지 않음
     + 즉, T3를 Standard 모드로 사용해 생성할 경우, 바로 Burst 진입 불가
<div align="center">
<img src="https://github.com/user-attachments/assets/5a8853d2-c627-4aef-b27a-215c37fd16d0" />
</div>

6. T2 인스턴스가 크레딧이 없을 경우
   - T2 인스턴스(Standard Mode)가 크레딧이 없을 경우 발생하는 현상
     + 인스턴스 응답이 없음
     + 애플리케이션이 자주 멈춤
       * 웹 서버의 경우 500 Error 등
       * 정상 동작(크레딧이 충분할 때)하다 멈춤(크레딧이 부족할 때) 반복
   - 해결책
     + 인스턴스 사이즈 늘리기
     + 인스턴스 타입 바꾸기 (예) M 타입)
     + Unlimited 모드로 바꾸기
     + 계획적으로 CPU 사용하기

7. 결론
   - T 시리즈 인스턴스(특히 T2) 타입의 경우 사용 계획이 필요
     + CPU 사용량에 대해 잘 계획해서 사용하면 이득을 볼 수 있지만, 막 사용하면 오히려 손해
     + 개발용 인스턴스 같은 경우 T타입이 적절
     + 라이브 서비스를 위한 인스턴스의 경우 T 타입을 사용할 때 주의가 필요
     + CPU Credit은 CloudWatch 지표로 모니터링 가능 : 알람 등 처리 가능
   - 사용량에 맞는 사이즈를 적절하게 사용 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/954599a1-411f-4fdb-9dc4-36e9a6d30188" />
</div>

