-----
### Placement Group
-----
1. Workload의 특성에 따라 EC2 배치를 조절하는 기능
2. 세 가지 종류
   - Cluster : 하나의 AZ안에 인스턴스를 최대한 가까이 배치
   - Partition : 서로 공유하지 않는 하드웨어 단위(파티션)로 인스턴스를 묶어 분산 배치
   - Spread : 최대한 하드웨어를 공유하지 않도록 분산 배치

3. Cluster Placement Group
   - 하나의 AZ안에 인스턴스를 최대한 가까이 배치 : 상호간의 Low Latency 네트워크가 필요한 경우 (예) HPC)
   - Group 안의 인스턴스는 서로 최대 10Gbps의 대역폭의 네트워크 통신 가능
     + ENA(Elastic Network Adapter) 활용 시 25Gbps
     + 인스턴스의 크기(vCPU의 숫자)에 따라 다른 대역폭 : 클수록 큰 대역폭 활용 가능
   - AZ 단위 : AZ 바깥으로 확장 불가능
<div align="center">
<img src="https://github.com/user-attachments/assets/7cbed3c0-b926-43ae-ad7f-956e9ccda4e5" />
</div>

   - 여러 인스턴스 타입 프로비전을 시도할 수 있으나 실패 확률 존재 : 단일 인스턴스 타입 권장

4. Partition Placement Group
   - 서로 공유하지 않는 하드웨어 단위(파티션)로 인스턴스를 묶어 분산 배치
   - 한꺼번에 다수의 인스턴스 실패가 발생하지 않도록 위치 분산이 목적
<div align="center">
<img src="https://github.com/user-attachments/assets/7ef4d767-fb4d-4566-ac2b-0cfc2fde5a97" />
</div>

   - 가용 영역 당 최대 7개의 파티션
     + 인스턴스를 자동으로 파티션별로 분산시키거나 특정 파티션에 프로비전 가능
     + EC2 프로비전 요청 시 하드웨어가 충분하지 않을 경우 요청 실패

   - 각 인스턴스가 어느 파티션에 위치해있는지 별도로 확인 가능 : 즉, HDFS, HBase, Cassandra 등 위치 정보 전달 가능

5. Spread Placement Group
   - 최대한 하드웨어를 공유하지 않도록 분산 배치
   - 작은 규모의 매우 중요한 인스턴스 클러스터의 안정성 확보 목적
<div align="center">
<img src="https://github.com/user-attachments/assets/79638ea0-1552-4c03-a066-bbca86e05669" />
</div>

   - 두 가지 종류
     + Rack Level : 단일 Rack 단위로 Rack 하나 당 하나의 인스턴스
       * AZ 당 7개의 Rack : AZ당 최대 7개의 인스턴스
       * 더 많은 인스턴스가 필요하다면, 여러 Spread Placement Group 활용 가능

     + Host Level : AWS Outpost 활용
       * AWS Outpost : AWS 서비스를 고객의 데이터센터로 확장하는 서비스
