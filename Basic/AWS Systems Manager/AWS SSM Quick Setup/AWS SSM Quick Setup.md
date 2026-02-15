-----
### AWS SSM Quick Setup
-----
1. AWS SSM의 기능들을 활용해서 가장 모범이 되는 방법으로 노드들을 빠르게 관리할 수 있는 서비스 (즉, 자주 활용되는 패턴 및 관리 절차를 모아둔 서비스)
2. 장점
   - 다수의 계정 / Organization 안에 있는 다수의 관리 노드에 적용 가능
   - 모범 사레들을 모아뒀으므로 복잡도 감소
   - 각 사용 사례를 위한 UI 제공
   - 주기적으로 Drift를 감지해서 처리
3. 내부적으로 CloudFormation 활용하므로, 권한 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/261b44af-8ecd-4e28-af64-bbaab3082475" />
</div>

4. 주요 기능 예시
   - SSM Agent 주기적 업데이트
   - 30분마다 서버 인벤토리 (Application, OS, Network 정보 등) 수집
   - 하루마다 시스템 / 애플리케이션의 누락 패치 확인 및 패치 적용
   - CloudWatch Agent 설정
   - EC2 자동 실행 및 중지
   - AWS 서비스 설정
     + AWS Config
     + DevOps Guru
     + Resource Explorer
<div align="center">
<img src="https://github.com/user-attachments/assets/32969197-1345-433b-8f34-a8cfd9f992ac" />
</div>

5. SSM의 이해를 바탕으로 설정 과정을 간소화하고 싶은 유저에게 적합 : 서비스를 처음 사용하거나 쉽게 사용하고 싶은 사용자 대상이 아님
6. Demo - EC2 자동 실행 및 종료
   - AWS SSM Quick Setup을 활용해 비용 절감을 위한 EC2 자동 시작 / 중지
     + 특정 시각(예) 오전 8시)에 EC2 시작 후, 특정 시각(예) 오후 7시)에 중지
     + 사용하지 않는 시간 동안에는 EBS 비용만 발생
   - 과정
     + SSM 인스턴스 역할 생성
     + EC2 프로비전
     + SSM QuickSetup으로 설정 및 인스턴스 중지 / 시작 테스트
     
