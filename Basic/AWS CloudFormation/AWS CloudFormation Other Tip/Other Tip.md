-----
### 콘솔에서는 숨겨진 리소스들
-----
1. CloudFormation에서 프로비전 시 콘솔 프로비전에서 보이지 않는 리소스 존재
2. 주로하나의 리소스와 다른 리소스를 연결해주는 숨겨진 리소스
3. 예) Instance Profile : IAM 역할과 EC2를 연결 / EBS 볼륨 맵핑
<div align="center">
<img src="https://github.com/user-attachments/assets/30d65ea8-9cb2-4884-a656-92277d417d77" />
</div>

-----
### 유용한 기능 - CloudFormation 가져오기
-----
1. 기존에 있는 리소스를 만들어진 CloudFormation 논리적 리소스(스택)으로 가져오기 가능 : 스택 간 리소스 옮기기도 가능
2. 두 가지 방법
   - IaC Generator : 기존 리소스를 기반으로 자동으로 템플릿을 생성해주는 서비스 (비추천
   - 수동 가져오기 : 수동으로 기존 리소스에 해당하는 리소스를 정의하여 가져오는 방법

-----
### 유용한 기능 - Drift Detection
-----
1. Drift : CloudFormation의 논리적 리소스와 실제 물리적 리소스가 다른 경우 : 주로, 물리적 리소스에 CloudFormation 이외의 수단 (SDK / Console / CLI)으로 변경한 경우
2. CloudFormation에서 스택의 Drift Detection 지원
   - 즉, 현재 상태기반으로 실제 리소스를 모니터링하여 변경점 확인
   - 현재는 Retain Delete 가져오기로만 해결 가능

-----
### LLM 활용
-----
1. CloudFormation도 문서이므로 LLM을 활용해 편리하게 작성 가능
2. 리팩토링 / 주석 / 추가 리소스 등
