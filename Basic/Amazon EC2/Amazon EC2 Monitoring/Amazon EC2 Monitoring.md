-----
### EC2 모니터링 기초
-----
1. Amazon CloudWatch
   - DevOps 엔지니어, 개발자, SRE(사이트 안정성 엔지니어) 및 IT 관리자를 위해 구축된 모니터링 및 관찰 기능 서비스
   - 애플리케이션을 모니터링하고, 시스템 전반의 성능 변경 사항에 대응하며, 리소스 사용률을 최적화하고, 운영 상태에 대한 통합된 보기를 확보하는 데 필요한 데이터와 실행 가능한 통찰력을 제공

2. Amazon CloudWatch와 EC2
   - AWS에서 제공하는 AWS 서비스 / 애플리케이션의 모니터링 서비스
   - EC2를 포함한 AWS의 다양한 서비스에 기본적 모니터링 지원
   - EC2의 주요 지표 (시간 순서로 정리된 데이터의 집합)
     + CPU 사용량
     + 디스크 (Read, Write 등)
     + 네트워크 (In, Out, Packet 등)
     + CPU Credit 관련 지표
     + 상태 관련 (Failed 등)

   - 이 외에 커스텀 지표(메모리 사용량 등) 활용 가능
   - EC2의 경우 별도로 활성화 불필요 (무료)
   - 기분 5분 단위 (상태 확인만 1분 단위)

   - EC2 세부 모니터링
     + 모든 지표에 1분 단위 모니터링
     + Metric 별로 비용 발생 (단, 저장 비용 없음)
     + 별도로 활성화 필요

3. Amazon CloudWatch와 Auto-Scale
   - Auto-Scale 지표 : Auto-Scale 관련 지표의 경우 별도로 활성화 필요
   - 주요 지표
     + 용량 (Min / Max / Desired)
     + 인스턴스 상태 (서비스 중, 종료, 멈춤 등)
     + 기타 관리중인 인스턴스 모두 통합 지표 제공

   - 이 외 내부적으로 Auto-Scaling의 알람, 타겟 추적 스케일링 정책 등은 CloudWatch 기반

4. Amazon CloudWatch와 ELB
   - ELB 지표는 기본적으로 활성화
   - 주요 지표
     + 요청 / 연결
     + Status Code 별 Count (3xx, 4xx, 5xx)
     + IPv6 처리
     + 네트워크 지표 (처리된 네트워크 트래픽)
   - 기타 지표 등 문서 참고
     + EC2 지표 목록 : ```https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html```
     + ELB 지표 목록 : ```https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-metrics.html```
     + ALB 지표 목록 : ```https://docs.aws.amazon.com/autoscaling/ec2/userguide/viewing-monitoring-graphs.html```

5. 실습
   - EC2 접속 - 시작 템플릿 생성 (이름 : demo-my-monitoring-test / AMI : Amazon Linux 2023 kernel-6.1 AMI) - 인스턴스 유형 : t2.micro - 키 페어 (시작 템플릿에 포함하지 않음) - 네트워크 설정 : 보안 그룹 (Default)
   - 고급 세부 정보 : 세부 CloudWatch 모니터링 활성화 가능
   - 템플릿 선택 후 작업 - 템플릿으로 인스턴스 시작 - 키 페어 없이 계속 진행
   - 생성된 인스턴스에서의 모니터링 메뉴 (기본적으로 EC2 인스턴들이 제공하는 지표 / 메모리는 제외되어 있음)
   - 기본 모니터링 시간은 5분이므로, 5분 이후 Metric 수집 - 세부 모니털 오간리 - 세부 모니터링 활성화 (1분 단위로 가능)
   - 시작 템플릿 추가 버전 생성
     + 작업 - 템플릿 수정 (새 버전 생성) - 세부 CloudWatch 모니터링 활성화 - 버전 하나 더 생성
     + Auto-Scaling 그룹 생성 : demo-monitoring-asg / 시작 템플릿 : demo-my-monitoring-test, 버전 Latest(2) 선택 / 가용 가능한 서브넷 모두 선택 / 원하는 용량 3개로 선택 / 모니터링 - CloudWatch 내에서 그룹 지표 수집 활성화 선택 / 태그 - Name, Demo-Monitoring
     + 생성후 ASG 모니터링 탭 (Auto Scaling / EC2 존재)
   - 자원 정리
     + Auto-Scaling 삭제
     + EC2 인스턴스 삭제
