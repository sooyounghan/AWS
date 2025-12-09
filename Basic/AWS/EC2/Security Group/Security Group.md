-----
### 보안 그룹
-----
1. EC2 그룹
<div align="center">
<img src="https://github.com/user-attachments/assets/20042e15-41b8-426f-a369-3670c332ee08" />
</div>

2. 보안 그룹 : 인스턴스에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽 역할
3. 즉, EC2의 방화벽 역할을 하는 서비스
4. Port 서비스
   - 기본적으로 모든 포트는 비활성화
   - 선택적으로 트래픽이 지나갈 수 있는 Port와 Source를 설정 가능
   - Deny는 불가능

5. 인스턴스 단위
   - 하나의 인스턴스에 하나 이상의 보안 그룹 설정 가능
   - 인스턴스에 여러 보안 그룹이 적용될 경우, 모든 보안 그룹 규칙을 적용 받음
<div align="center">
<img src="https://github.com/user-attachments/assets/eab4e3b1-fd24-4c6d-9bf3-52291b1463fd" />
</div>
