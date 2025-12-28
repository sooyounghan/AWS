-----
### AutoScale Scaling 정책
-----
1. Autoscale에서 관리하고 있는 인스턴스의 숫자를 조절하는 방식
2. 네 가지 분류
   - 수동 스케일 : 수동으로 직접 인스턴스 숫자를 증감
   - 스케줄 기반 스케일 : 특정 시점에 인스턴스 숫자를 증감 (주로 예측 가능한 시점의 부하 처리 목적으로 활용)
   - 동적 스케일 : 특정 기준을 두고 기준치에 따라 인스턴스의 숫자를 증감 (예) CPU 사용률 기반, 요청 숫자 기반, 판매량 기반 등)
   - 예측 기반 스케일 : 과거의 기록의 패턴을 기반으로 수요량을 예측해서 인스턴스 숫자를 증감

3. 수동 스케일
   - 말 그대로 수동으로 인스턴스 숫자를 조절하는 방법 : 주로 개발 환경 혹은 다른 정책을 적용하기 전 사전 테스트 용도로 활용
   - 되도록이면 다른 스케일 정책을 비활성화 시킨 후 적용 추천

4. 스케줄 기반 스케일링
   - 예측 가능한 시점의 변동사항에 대비해서 인스턴스 숫자를 조절하는 방식 (예) 매주 수요일마다 주간 이벤트가 있는 게임의 경우 / 매일 새벽 특정 시간에 하루동안 모인 데이터를 분석)
   - 적용 방식
     + 특정 시점을 정해서 해당 시점 또는 Cron으로 표현되는 반복 시점에 범위 지정 (Min, Max, Desired 중 최소 한 가지)
     + 지정한 범위보다 인스턴스가 작다면 Scale-Out, 크다면 Scale-In
<div align="center">
<img src="https://github.com/user-attachments/assets/23479f23-d305-4631-9e82-d767266647db" />
</div>

   - Cron Expression
     + 특정 시간의 주기를 표현하기 위한 표현식
     + 1분 단위가 최소 단위
<div align="center">
<img src="https://github.com/user-attachments/assets/71231c87-fbd8-4119-adc2-5f9834d5c044" />
<img src="https://github.com/user-attachments/assets/a89790c7-e70e-4af0-a57c-86fc49beadf3" />
<img src="https://github.com/user-attachments/assets/8573063d-54b9-4384-af56-49d398ff7de8" />
<img src="https://github.com/user-attachments/assets/22408af9-22e6-470b-bd3d-0f28c01e8ef9" />
<img src="https://github.com/user-attachments/assets/11369535-d52c-40ff-be5e-51c014e12d0c" />
</div>

5. 동적 스케일
   - 지표에 반응해서 인스턴스 숫자를 조절하는 방식 : 주로 CloudWatch의 지표 활용 혹은 자신만의 기준으로 스케일링 정책 조절 가능
   - 주로 Autoscale에서 지원하는 추적 조정 정책(Target Tracking Policy) 활용
     + 내부적으로 CloudWatch 경보(Alarm)을 생성해서 경보에 반응하여 자동으로 증감 (경보 : CloudWatch 지표에 반응하여 트리거되는 이벤트)
     + 지표 증감에 따라 얼마나 민감하게 반응할 것인지 결정 가능
   - 필요 시 커스텀 로직을 활용해 Autoscale의 증감을 수동으로 조절 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/02e11ea1-da08-45fa-a73a-e1fb6437055d" />
</div>

6. 예측 기반 스케일
   - 사용 패턴의 히스토리를 기반으로 인스턴스 수요량을 예측하여 숫자를 조절하는 방식
     + 주로 반복되는 패턴이 명확한 경우, 혹은 인스턴스 준비가 오래 걸리는 경우 사용
     + 예) 매일 오후 2시에 레이드 이벤트가 열리는 게임 서비스의 서버

   - CloudWatch 지표를 14일 간 분석하여 패턴을 만들고, 다음 48시간 사용 패턴 분석
   - 수요량에 따라 증가만 가능 : 즉, 인스턴스 숫자를 감소시키려면 동적 스케일링 정책 필요
   - 주의사항
     + AutoScale Group에 다양한 인스턴스 종류가 섞여있을 경우 효율성이 매우 떨어짐
     + 다양한 스케일링 정책이 공존할 경우 효율성이 떨어짐
     + AutoScale Group에 충분한 데이터가 쌓일 때까지 (2주) 기다림 필요

7. Demo - 동적 스케일링 확인
   - EC2 시작 템플릿 : demo-autoscale-policy-template / default 보안 그룹 설정 / 고급 세부 정보 - 세부 정보 CloudWatch 활성화
   - AutoScale Group에서 인스턴스 프로비전 : 세부 모니터링 활성화
     + 동적 스케일링 정책 중 타겟 추적 정책 활성화 : CPU Utilization 추적
     + Auto-Scaling 그룹 생성 : demo-autoscale-policy
     + 가용 영역 및 서브넷 모두 선택
     + 원하는 최대 용량 3 및 대상 추적 크기 조정 정책 선택 - 평균 CPU 사용률 지표 선택 (대상 값 : 50 - 50% 이상 일경우)
     + 추가 설정 - CloudWatch 내에서 그룹 지표 수집 활성화
     + 태그 : Name - Demo-Autoscale-Policy

   - 인스턴스에 강제로 부하를 발생시켜 CPU 사용량을 100%으로 유지
```bash
sudo -s
dnf install stress -y # CPU 사용률 100%까지 증가
stress --cpu 1 --timeout 300 # 300초 동안 100% 증가, 이후 감소
```

   - 이후 스케일링 확인 (Auto-Scaling 그룹 - 활동)
   - 주의사항 : Scale-Out은 3분 정도 지만, Scale-In은 15분 이상 소요
