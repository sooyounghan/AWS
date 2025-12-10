-----
### EC2의 생명 주기
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/b128323f-4947-418f-8989-c07e3e55a17d" />
</div>

1. 중지
   - 중지 중에는 인스턴스 요금 미청구
   - 단, EBS 요금 / 다른 구성 요소(Elastic IP 등)은 청구
   - 중지 후 재시작 시, Public IP 변경
   - EBS를 사용하는 인스턴스만 중지 가능 : 인스턴스 저장 인스턴스는 중지 불가
   - 인스턴스의 종류
<div align="center">
<img src="https://github.com/user-attachments/assets/58f8cc67-b5b7-4e6e-8344-2aff1e982114" />
</div>

2. 재부팅 : 재부팅 시에는 Public IP 변동 없음
3. 최대 절전모드 : 메모리 내용을 보존해서 재시작 시 중단 지점에서 시작할 수 있는 정지 모드
<div align="center">
<img src="https://github.com/user-attachments/assets/bb6ef470-e3d0-4f32-ae28-cbcfdbca16f1" />
</div>

4. 실습 : EC2 중지 및 재부팅
   - 인스턴스 생성 : demo-ec2
   - 중지 후 재실행 시 IP 주소가 바뀌는 것 확인
   - 재부팅 시 IP 주소가 바뀌지 않는 것 확인
