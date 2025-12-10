-----
### ENI (Elastic Network Interface)
-----
1. 탄력적 네트워크 인터페이스 : VPC에서 가상 네트워크 카드를 나타내는 논리적 네트워킹 구성 요소
2. EC2의 가상의 LAN 카드
3. IP 주소와 MAC 주소 보유
4. 하나의 인스턴스에 여러 개 ENI 연도 가능 : 하나의 인스턴스가 한 개 이상의 IP 보유 가능
5. 인스턴스 유형 및 사이즈에 따라 최대 보유 가능한 IP 주소가 변동
6. 내부적으로는 보안 그룹은 ENI에 부착
7. 기본적으로 Private IP와 Private Domain 보유 : 선택적으로 Public IP와 Public Domain 보유
<div align="center">
<img src="https://github.com/user-attachments/assets/b905151f-f67e-47a9-863e-fe3a76e0f29e" />
<img src="https://github.com/user-attachments/assets/b39d5fec-fa2a-488c-a13f-44e5d59558e0" />
</div>

-----
### 탄력적 IP (Elastic IP)
-----
1. EC2의 Pulbic IP를 고정해주는 서비스 : 즉, 인스턴스를 중지에서 재시작해도 고정적 IP 확보 가능
2. EC2 이외에 다른 서비스(예) NLB)에도 사용
3. 내가 보유한 IP 주소를 AWS에서 사용 가능
4. Region 단위
5. 연결하지 않아도 보유하기만 해도 비용 발생 (IPv4 비용 발생)

-----
### 실습
-----
1. EC2 생성 : demo-ec2-elastic-ip / 키 페어 필요 없음
2. Elastic IP 설정 후 중지, 이후 재시작 시 IP 변동 없음 확인
   - 좌측 네트워크 - 탄력적 IP - 탄력적 IP 주소 할당 - 생성 탄력적 IP 선택 후 탄력적 IP 주소 연결 - 인스턴스 ID 값 입력 후 연결
3. EC2 종료
4. Elastic IP 정리 : 작업 - 탄력적 IP 주소 연결 해제 - 연결 해제 - 탄력적 IP 주소 릴리스
