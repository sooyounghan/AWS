-----
### ELB + ASG 실습
-----
1. 실습 - ALB + Auto-Scaling
   - 시작 템플릿 생성 : 사용자 데이터(User Data) 스크립트 활용 웹 서버 설치 및 인스턴스 아이디 표시
     + 시작 템플릿 이름 : demo-my-ec2-template
     + AMI : 더 많은 AMI 찾아보기 (Amazon Linux 2023 kernel-6.1 AMI 선택)
     + 인스턴스 유형 : t2.micro
     + 키 페어 : demo-my-ec2-keypair
     + 보안 그룹 생성 : demo-my-sg (인바운드 보안 그룹 규칙 : 유형 - 모든 트래픽 / 소스 유형 - 위치 무관)
     + 고급 세부 정보 : IAM 인스턴스 프로파일 (demo-iam-role-for-ec2)
     + 사용자 데이터 입력
```bash
#!/bin/bash
sudo -s
dnf install httpd -y
service httpd start
chkconfig httpd on
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
echo "$INSTANCE_ID" >> /var/www/html/index.html
```

   - Auto-Scaling Group 생성 및 테스트
     + Auto-Scaling Group 생성 : demo-my-asg
     + 시작 템플릿 : demo-my-ec2-template
     + 버전 : Default(1)
     + VPC : 가용 영역 및 서브넷 모두 선택
     + 로드 밸런싱 : 로드 밸런서 없음 / 상태 확인 유예기간 : 300초
     + 그룹 크기 : 원하는 용량 / 원하는 최소 용량 및 최대 용량 (3) 설정 가능
     + 알림 추가 가능
     + 태그 설정 : Name / Demo-My-ASG
     + 활동 탭 : 어떠한 작업들이 발생되는지 볼 수 있음
     + 인스턴스 관리 탭 : 현재 실행되고 있는 EC2 인스턴스 확인 가능 
     + 원하는 용량 3개 설정 : 3개 인스턴스 생성 (변경 가능) - 2개로 변경되면, 모니터링하면서 다시 3개로 유지하도록 됨

   - 대상 그룹 생성 + ALB 생성
     + 대상 그룹 : 인스턴스 / 대상 그룹 이름 : demo-by-target-group / 상태 검사 경로 : /index.html / 고급 상태 검사 설정 - 정상 임계 값 : 2, 간격 : 5초 (5초 * 2번 = 10초) / 제한 시간 : 2초 (간격보다 적어야 함)  
     + ALB 생성 : demo-my-alb / 가용 영역 서브넷 모두 선택 / 보안 그룹 : default / 리스너 : 포트 80, 대상 그룹 선택 : demo-by-target-group
        * 보안 : 보안 그룹이 현재 외부에서 트래픽을 받을 수 없으므로, In-Bound에서 모든 소스에서 트래픽을 받도록 설정 (인바운드 규칙 편집 - 기존 규칙 삭제 / 규칙 추가 - 모든 트래픽 / 0.0.0.0/0 설정)

   - ALB와 연결 및 테스트
     + Auto-Scaling 그룹 선택 - 통합 - 로드 밸런싱 편집 - 애플리케이션, 네트워크 또는 게이트웨이 로드 밸런서 대상 그룹 선택 (demo-my-target-group)
     + 편집 - 원하는 용량 3개로 증가 

   - Auto-Scaling Group의 인스턴스 증감에 따른 ALB 연동 확인
     + 로드 밸런서 - DNS 이름 복사 후 확인 : 3개의 인스턴스에 트래픽 분배
     + Auto Scaling 그룹 - 세부 정보 - 상태 확인 - 추가 상태 확인 유형 - Elastic Load Balancer 상태 확인 켜기 선택 (ALB가 애플리케이션이 잘 동작하는지 확인하도록 하고, 그 결과를 Auto-Scaling 그룹에 반영해 EC2 인스턴스 숫자로 업데이트 하는 것)
     + 한 EC2 인스턴스 접속 후, 웹 서버 중지 : sudo -s / service httpd stop (대상 그룹에서도 비정상으로 판단)
     + 대상 그룹으로 확인하면, Unhealthy / Health checks failed 상태
     + Auto-Scaling에서도 확인 가능 (Unhealthy이면, 새로운 EC2 인스턴스 생성 / 여유 시간 (등록 취소 지연) 동안 존재하다가 삭제
       
   - Listner 간단 테스트
     + 로드 밸런서 - 리스너 및 규칙 - 리스너 관리 - 리스너 편집 - 포트 8080으로 변경 (ALB는 8080 포트로 받아 80 포트로 넘겨줌)

   - 💡 리소스 정리 (Connection Draining)
     + 로드 밸런서 삭제
     + Auto Scaling 그룹 삭제 또는 원하는 용량을 0으로 삭제
     + 대상 그룹 삭제
    
