-----
### NACL (Network Access Control List)
-----
1. 1개 이상 서브넷 내부와 외부의 트래픽을 제어하기 위한 방화벽 역할을 하는 VPC를 위한 선택적 보안 계층
2. 보안 그룹처럼 방화벽 역할을 담당
3. 서브넷 단위
   - 즉, 인스턴스 단위로 제어 불가능
   - 다양한 서브넷에 연동 가능 (1 : N)

4. Port 및 IP를 직접 Deny 가능 : 외부 공격을 받는 상황 등 특정 아이피를 Block하고 싶을 때 사용
5. Stateless
   - 들어오는 트래픽과 나가는 트래픽을 구분하지 않음
   - 즉, 일반적으로 Out-Bound에 Ephemeral Port(임시 포트) 범위를 열어줘야 정상적으로 통신 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/7028e6ae-b86d-4f36-985d-06d28e9b60d0" />
</div>

6. 규칙
   - 💡 규칙 번호 : 규칙에 부여되는 고유 숫자이며 규칙이 평가되는 순서 (낮은 번호부터 평가)
     + AWS 추천은 100단위 증가 (규칙 사이에 들어올 수 있는 가능성이 있으므로 일반적으로 100단위 설정)
     + 한 번이라도, 평가가 되면 종료
   - 유형 : 트래픽 유형 (예) SSH=22, DNS=53, UDP=17 등)
   - 프로토콜 : 통신 프로토콜 (예) TCP, UDP, SMP 등)
   - 포트 범위 : 허용 혹은 거부할 포트 범위
   - 소스 : IP 주소의 CIDR 블록
   - 허용 / 거부 : 허용 혹은 거부 여부
   - AWS NACL 규칙 순서
<div align="center">
<img src="https://github.com/user-attachments/assets/fd94aafb-a466-4f84-9781-1f6c025fde85" />
<img src="https://github.com/user-attachments/assets/ee5bd111-16cc-4cca-aad4-54160b8a84ff" />
<img src="https://github.com/user-attachments/assets/ba897844-0a32-4cd1-bbca-83763c86c5a2" />
</div>

7. 고려 사항
   - Stateless 방화벽이므로 원활한 통신을 위해서 OutBound도 신경써야 함
     + 임시 포트 Allow 필요
     + Linux : 32768 ~ 61000
     + Windows : 1025 ~ 5000(XP), 49152 ~ 65535 (Vista 이상)
   - 서브넷에서 나가거나 들어오는 트래픽에만 적용 : 즉, 서브넷 내부의 트래픽에 대해서는 적용되지 않음
   - 다양한 서브넷으로 구성되어 있는 Multi-Tier 아키텍쳐라면 더 많은 고민이 필요
     + 데이터베이스가 응답을 보내는 트래픽도 임시 포트 Range 설정도 고려해야함 
     + 따라서, 가능한 보안 그룹으로 먼저 처리 후 꼭 필요할 때만 NACL 활용할 것
<div align="center">
<img src="https://github.com/user-attachments/assets/43c7a520-32f1-4a7e-9355-9d80aa5ce5e5" />
</div>

   - VPC 생성 시, 혹은 AWS 계정 생성 시 주어지는 VPC에 기본 하나 제공
     + 기본으로 만들어진 NACL에는 모든 트래픽 Allow 되어있음
     + 단, 직접 생성하는 NACL의 경우 모든 트래픽 Deny
   - 💡 하나의 서브넷은 하나의 NACL만 연동 가능
   - 💡 단, 하나의 NACL는 여러 서브넷에 연동 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/881e1523-3871-481a-921e-f551877b4161" />
<img src="https://github.com/user-attachments/assets/af8d76a8-1593-4654-93ac-4bce2184fedc" />
</div>

8. Demo
   - VPC - 보안 - Network ACL
     + 기본적으로 존재하는 NACL는 규칙번호 100에 인바운드 / 아웃바운드 규칙이 모든 트래픽에 대해 Allow, *에 대해서는 Deny
     + 인바운드 규칙 편집 - 규칙 번호 200 - SSH - 거부 - 변경 사항 저장
     + 인스턴스 연결을 통해 SSH 연결 (연결 됨 : 규칙 번호에서 100에 의해 먼저 평가되고 종료)
     + SSH를 거부하고 싶다면, 규칙 번호로 50으로 둔다면, SSH 트래픽 (22번 포트)에서 오는 트래픽은 거부됨
     
   - NACL 아웃바운드 규칙 편집 - 모두 거부 - 더 이상 통신이 불가 (Timeout)
