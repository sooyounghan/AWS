-----
### AutoScale 기타 기능
-----
1. Health Check
   - 인스턴스의 상태를 체크하는 기능
   - 상태 확인 소스
     + EC2 : 인스턴스가 실행 중인지, 하드웨어 상태가 정상적인지 확인
     + ELB : 트래픽을 발생시켜서 해당 트래픽을 잘 수행하는지 확인
     + VPC Lattice 
     + 커스텀 : 직접 커스텀 로직으로 상태를 확인

   - 상태 확인을 실패했을 때 Unhealthy 상태로 변경 : AutoScale에서 직접 해당 인스턴스를 교체

2. 인스턴스 Warm Up / Health Check Grace Period
   - 인스턴스 Warm Up
     + 인스턴스에 대한 CloudWatch의 모니터링 지표가 수집 되기 전 준비 기간
     + 트래픽을 받기 전 지표 등이 반영되지 않도록 설정 가능

   - Health Check Grace Period
     + 신규로 추가된 인스턴스가 Health Check를 수행하기 전 준비를 위한 기간 : 기본 300초 (콘솔 기준, CLI/SDK는 0초)
     + 인스턴스가 신규로 올라간 이후 준비가 오래 필요한 경우 등 활용

3. Instance Scale-In Protection
   - AutoSacle 혹은 인스턴스 단위로 인스턴스 종료를 막는 기능
   - 디버그 목적 혹은 특정 로직을 끝까지 수행할 수 있도록 보장하기 위해 사용 (예) SQS에서 받아온 데이터를 처리하는 특정 로직이 끝까지 수행될 수 있도록 보장하기 위해 해당 로직이 완료 전까지 Protection을 활성화)
   - Protection이 있어도 종료되는 경우
     + Health Check Fail
     + Spot 인스턴스
     + 수동으로 삭제 (콘솔, CLI/SDK)

4. 인스턴스 Lifetime 관리
   - 인스턴스가 최대로 실행되어 있는 기간 (초 단위)을 설정 가능 : 해당 기간 이후 자동으로 교체(Replace)
   - 최소 86,400초 (1일)
   - 설정을 취소하려면 새로운 값을 0으로 입력 : 모든 인스턴스에 적용
   - Scale In Protection이 걸려있을 경우, 해당 기능 우선 : 즉, 인스턴스가 종료되지 않을 수 있음
   - 기본 교체 방법은 인스턴스가 종료된 후 신규 인스턴스 생성

5. 인스턴스 유지 관리 정책 : 인스턴스의 숫자가 변동할 때, 어떤 우선순위로 어떻게 변동할지 정하는 정책
   - 기본 정책
<div align="center">
<img src="https://github.com/user-attachments/assets/b27728c5-b996-4416-9465-b1bec2380860" />
</div>

   - 커스텀 정책
<div align="center">
<img src="https://github.com/user-attachments/assets/2318171b-70d5-431b-b2c3-7ad8ce2ef384" />
</div>

6. AutoScale 기능 임시 비활성화
   - 임시로 Autoscale의 다양한 기능을 비활성화 / 활성화 가능 : 주로 디버그 / 상황 대응의 목적으로 활용
   - 주요 비활성화 가능한 기능
     + Launch : 인스턴스의 Scale-Out이 필요할 때, 신규 인스턴스를 시작을 비활성화
     + Terminate : 인스턴스의 Scale-In이 필요할 때(Health Check Fail 포함), 인스턴스 중지를 비활성화
     + AddToLeadBalancer : ELB에 인스턴스 등록 중지
     + AZReblance : 가용 영역 간 골고루 인스턴스가 분배되도록 인스턴스 조절 (신규 프로비전 / 종료 포함) 기능을 중지
     + HealthCheck : 인스턴스 상태 체크(ELB 포함)을 비활성화
     + ReplaceUnhealthy : HealthCheck에 실패한 인스턴스 교체 비활성화

7. AutoScale 관리 임시 해제
   - 임시로 AutoScale으로 관리중인 인스턴스를 StandBy 상태로 전환 가능
     + StandBy : ELB의 트래픽을 받지 않고, HealthCheck도 임시로 비활성화된 상태
   - 업데이트, 디버그 등 목적으로 활용 가능 (예) 인스턴스를 StandBy로 두고, 관련 소프트웨어 업데이트 후 복귀)
   - StandBy 설정 시, desired capacity 조절 가능
     + desired capacity 하나를 낮추면, AutoScale 그룹에서 인스턴스 숫자 유지
     + desired capacity 변경 없이 진행된다면, AutoScale 그룹에서 인스턴스 추가
   - AZRebalance가 비활성화되어있지 않다면, StandBy가 풀리면서 인스턴스 조정 가능

8. AutoScale LifeCycle Hooks
   - AutoScaling의 각 단계(Pending, Terminating)에서 특정 로직을 수행할 수 있는 기능
   - 미리 수행 되어야 하는 작업 또는 CI / CD 파이프라인을 통한 배포 등을 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/43b2be8c-4daf-41b2-823b-3c896e3174fb" />
</div>

   
