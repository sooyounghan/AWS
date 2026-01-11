-----
### 보안 그룹 (Security Group)
-----
1. 인스턴스에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽 역할
2. Network Access Control List(NACL)와 함께 방화벽 역할을 하는 서비스
   - Port 허용
     + 기본적으로 모든 포트는 비활성화
     + 선택적으로 트래픽이 지나갈 수 있는 Port와 Source를 설정 가능
     + Deny는 불가능 → NACL로 가능

   - 인스턴스 단위 (정확히는 ENI 단위)
     + 하나의 인스턴스에 하나 이상의 보안 그룹에 설정 가능 : 설정된 인스턴스는 설정한 모든 보안그룹의 룰을 적용 받음
     + NACL의 경우 서브넷 단위

   - 기본적으로 VPC 단위 : VPC 안에서 생성하고 관리
     + 단, VPC 간 보안그룹 공유 기능으로 여러 VPC에서 사용 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/426bc26f-7c29-40be-84fe-e7b4d3ebbf7e" />
<img src="https://github.com/user-attachments/assets/df69c16e-9a5d-45ad-a8c0-99d753766c30" />
</div>

-----
### Stateful, Stateless
-----
1. Stateful
   - 보안 그룹은 Stateful
   - In-Bound로 들어온 트래픽이 별 다른 Out-Bound 설정 없이 나갈 수 있음
   - NACL은 Stateless
<div align="center">
<img src="https://github.com/user-attachments/assets/3af1e876-821e-4d27-9983-38abcb06809f" />
<img src="https://github.com/user-attachments/assets/85da9596-03ae-4c38-a3bf-b16352deeac3" />
</div>

2. Stateless
<div align="center">
<img src="https://github.com/user-attachments/assets/0fa7a041-7630-46e5-8fd0-1497427aeadb" />
</div>

-----
### 보안 그룹의 Source
-----
1. IP Range(CIDR)
2. 접두사 목록 (Prefix List)
   - 하나 이상의 CIDR 블록 집합
   - 보안 그룹 혹은 Route Table에 많은 대상을 참조하기 위해 사용
   - 두 가지 종류
     + 고객 관리형 : 직접 IP 주소를 생성 / 수정 / 삭제할 수 있으며, 다른 계정과도 공유 가능
     + AWS 관리형 : AWS의 서비스들을 위한 IP 목록으로, 수정 / 삭제 / 업데이트가 불가능함 (DynamoDB, S3, CloudFront)
   - IPv4, IPv6 둘 다 사용 가능 (단, 한 접두사 목록에 두 가지 타입을 동시에 사용 불가능)
   - 생성 시점에 최대 엔트리 숫자 지정 (이후 변경 가능)
   - 관리형 접두사 목록 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/ba3e1bf8-5447-4558-8443-5f3f6bcd7c74" />
</div>

3. 다른 보안그룹 (보안그룹 참조)
<div align="center">
<img src="https://github.com/user-attachments/assets/782e5993-7406-401f-8797-7adb7f2cab8c" />
<img src="https://github.com/user-attachments/assets/96ac2f05-3db2-457d-9171-175bb559e908" />
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/32f7b1e0-cbf2-4caf-8352-0746b892a6df" />
</div>

4. 보안 그룹 및 접두사 목록
<div align="center">
<img src="https://github.com/user-attachments/assets/6b6798f6-dc17-475b-9ac3-4d1faaead60b" />
<img src="https://github.com/user-attachments/assets/4f767f21-53ff-47fb-b2fc-4b0650722b56" />
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/8e05c8ec-7dcd-4811-be33-b5620283ef03" />
<img src="https://github.com/user-attachments/assets/2630b59e-66c2-4e94-abac-a997cb720909" />
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/7dd7f7d8-b200-4ac7-a1c2-602a5800be89" />
<img src="https://github.com/user-attachments/assets/74d8f60a-03ff-4f5b-8360-6a857f0687a6" />
</div>

5. Demo - 보안그룹 참조
<div align="center">
<img src="https://github.com/user-attachments/assets/2fab4f07-c13c-4621-8d2c-e005eb7ad99e" />
</div>
