-----
### AWS 계정 (Account)
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/355eeb5d-a716-4a36-9fca-3cee26b86007" />
<img src="https://github.com/user-attachments/assets/98dc3bf7-a789-4196-8e6d-bf7c7a23fec3" />
<img src="https://github.com/user-attachments/assets/b7d766bf-1cdd-447f-b49f-e28a8db44a26" />
</div>

-----
### 계정 분리의 장점
-----
1. 보안 및 안정성 : 하나의 계정에 문제(해킹, 리소스 삭제, 장애)이 생겨도 다른 계정과 격리되어 통제 가능 (Blast Radius)
2. 비용 관리 : 계정 단위로 비용을 확인 및 관리 가능
3. 관리 효율 : 각 계정별로 리소스 관리, 권한 할당 등의 단위 분리 가능
   - 예) 로그 관리 전용 계정
   - 예) dev → stage → live 등
<div align="center">
<img src="https://github.com/user-attachments/assets/1c0f5aa6-69a5-4794-94d3-463dc62d6137" />
<img src="https://github.com/user-attachments/assets/a481ec01-810e-4790-92ae-77e0e556116e" />
<img src="https://github.com/user-attachments/assets/8b1b4810-84c1-41c4-8ca4-e8e82dcb4ba3" />
</div>

-----
### 교차 과정 Assume Role
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/e5fd1494-6676-450d-9847-e692a8a87651" />
</div>

1. IAM 계정으로 로그인 후 다른 계정으로 IAM 역할을 Assume 해서 해당 계정을 제어하는 방식
2. 조건
   - 원래 계정의 IAM 사용자가 다른 계정의 역할을 Assume 할 수 있을 것
   - 다른 게정의 역할이 원래 계정의 IAM 사용자가 사용할 수 있도록 할 것

3. 이후 원래 계정의 사용자는 다른 게정의 역할이 허용한 범위 내 권한 행사 가능
4. AWS 콘솔 UI에서 스위치 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/4cfdd888-62b3-494d-ae87-b84e644366e9" />
</div>
