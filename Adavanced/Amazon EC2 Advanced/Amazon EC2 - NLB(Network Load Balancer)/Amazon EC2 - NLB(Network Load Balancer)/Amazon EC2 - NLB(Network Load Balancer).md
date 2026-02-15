-----
### Network Load Balancer
-----
1. OSI Model Layer 4
   - TCP, UDP, TLS 등
   - HTTP 프로토콜 이해 불가능 : 즉, Header, Cooke, 세션 등 활용 불가능

2. ALB에 대비 장점
   - ALB에 비해 매우 빠른 속도 : 초당 수백만건 이상 요청 처리
   - 연결 지속 가능
   - Elastic IP 할용 가능 : IP 고정  가능

3. 주요 사용 사례 : 빠른 TCP / UDP 연결이 필요한 애플리케이션 : 게임 / SSH / 실시간성이 필요한 애플리케이션, 금융 등
<div align="center">
<img src="https://github.com/user-attachments/assets/ca69edd0-072d-4cbb-a265-b35026f11d2f" />
</div>

4. Network Load Balancer의 고정 IP 
   - 본래 ELB (Elastic Load Balancer)의 경우 DNS 기반 접속하며 프로비전 된 Node의 IP 목록 중 하나 전달
<div align="center">
<img  src="https://github.com/user-attachments/assets/abc5fa2c-4fbd-48a8-9e7e-795465982d90" />
</div>

   - NLB의 경우, 이 Node에 Elastic IP 고정 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/dd4017f3-edbc-4273-af45-030121386e52" />
</div>

   - 💡 이미 생성된 NLB에 EIP을 붙이고 싶을 경우 노드 재생성 필요 : 즉, 연동 서브넷을 제거하고 다시 생성 필요

-----
### Demo - NLB 프로비전 및 테스트
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/a565c27c-8b0d-42f1-9b6b-f60f550595b4" />
<img src="https://github.com/user-attachments/assets/0cfaf6a1-1202-42e3-ba11-6ef055b13172" /></div>

1. EC2 + Node.js 기반 TCP 서버를 CloudFormation으로 프로비전
   - 간단하게 TCP로 접속한 클라이언트에게 1초마다 현재 시각을 전달하는 서버
   - 3000 포트를 활용해 해당 포트를 여는 보안 그룹 역시 프로비전
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 - cloudformation.yml 업로드
     + 스택 이름 : demo-nlb-test

2. 대상 그룹 추가 및 프로비전한 EC2 추가
   - EC2
     + 대상 그룹 생성 - demo-my-nlb-tg / 프로토콜 : TCP (PORT 3000) / 상태 검사 : TCP (프로비전 된 EC2 추가)
     + 로드밸런서 - 네트워크 로드 밸런서 - demo-my-nlb - 가용영역 및 모두 서브넷 체크 - 보안 그룹 : default / 리스너 프로토콜 : TCP(Port 3000 / 대상 그룹 : demo-my-nlb-tg

   - 네트워크 로드 밸런서 DNS 이름 사용하여 client.js 부분 변경
```JS
// Default connection parameters
const DEFAULT_HOST = '3.37.75.125' // 네트워크 로드 밸런서 DNS 이름
const PORT = 3000;
```

3. Node.js 기반 클라이언트로 접속 확인
```
node ./src/client.js
```

   - EC2를 통해 로그 확인
```
sudo -s
dir
tail -f server.log
```

4. 이후 고정 IP를 추가하여 고정 아이피로 접속 확인
   - EC2 - 탄력적 IP 주소 할당
   - 네트워크 로드 밸런서 - 네트워크 매핑 - 서브넷 편집 - 서브넷 해제 (EC2 가용 영역 확인하여 해당 가용 영역에 대한 해제, 다른 가용 영역이면 오류 발생) - 변경 내용 저장 후 다시 진입 후 서브넷 활성화 (즉, 노드를 재생성)
     + 네트워크는 교차 영역 로드 밸런싱이 기본적으로 끔 상태
     + 속성 - 편집 로드 밸런서 대상 선택 정책 : 교차 영역 로드 밸런싱 활성화 확인 (EC2 가용 영역 확인하여 해당 가용 영역에 대한 해제, 다른 가용 영역이면 오류 발생 해결)

5. 리소스 정리 : 로드 밸런서 삭제 / CloudFormation 스택 제거
