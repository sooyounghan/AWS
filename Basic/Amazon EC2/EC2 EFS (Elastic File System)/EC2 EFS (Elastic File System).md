-----
### EC2 + Auto-Scaling
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/34efe3fc-0f3c-4430-afd0-a78126d2458f" />
</div>

-----
### Amazon EFS (Elastic File System)
-----
1. AWS 클라우드 서비스와 On-Premise 리소스에서 사용할 수 있는, 간단하고 확장 가능하며 탄력적인 완전 관리형 NFS 파일 시스템 제공
2. 애플리케이션을 중단하지 않고, On-Demand 방식으로 페타바이트 규모까지 확장하도록 구축되어, 파일을 추가하고 제거할 때 자동으로 확장하고 축소하며 확장 규모에 맞게 용량을 프로비저닝 및 관리할 필요가 없음
<div align="center">
<img src="https://github.com/user-attachments/assets/ec7e3ca2-2e98-4264-a9d3-71aca259739c" />
</div>

3. NFS 기반 공유 스토리지 서비스(NFSv4) : 따로 용량을 지정할 필요 없이 사용한 만큼 용량이 증가 (EBS는 미리 크기를 지정해야 함)
   - 페타바이트 단위까지 확장 가능
   - 몇 천개의 동시 접속 유지 가능
4. 데이터는 여러 AZ에 나누어 분산 저장
5. 쓰기 후 읽기(Read After Write) 일관성
6. Private Service : AWS 외부에서 접속 불가능
   - AWS 외부에서 접속하기 위해서는 VPN 혹은 Direct Connect 등으로 별도로 VPC와 연결 필요
7. 각 가용영역에 Mount Target을 두고, 각각의 가용 영역에서 해당 Mount Target으로 접근
8. Linux Only
<div align="center">
<img src="https://github.com/user-attachments/assets/66334b16-6279-4cc1-9025-b747dd3b255c" />
</div>

9. Amazon EFS 타입
    - Regional : Region 전체를 활용하여 고가용성과 내구성 확보
    - One-Zone : AZ 하나만 활용하여 고가용성과 내구성이 떨어지지만 저렴한 가격

10. Amazon EFS 퍼포먼스 모드
     - General Purpose : 가장 보편적인 모드로, 거의 대부분 사용 권장
     - Max IO : 매우 높은 IOPS가 필요한 경우 (빅데이터, 미디어 처리 등)

11. Amazon EFS Throughput 모드
    - Elastic Throughput(추천) : EFS가 자동으로 Throughput 조절 - 예측하기 어렵거나 적은 범위에서 변동이 있는 경우
    - Bursting Throughput : 낮은 Throughput일 때, 크레딧을 모아 높은 Throughput일 때 사용 (EC2 T 타입과 비슷한 개념)
    - Provisioned Throughput : 미리 지정한 만큼의 Throughput을 미리 확보해두고 사용

