-----
### ELB의 구조
-----
1. Internet-Facing : VPC 외부의 인터넷에서 직접 접근할 수 있는 ELB
   - Public IP를 가진 노드를 AZ내부에  생성하고 DNS로 접근
   - 주로 외부의 요청에 대한 트래픽을 분배할 때 사용

2. Internal : VPC 내부에서만 접근할 수 있는 ELB
   - Private IP만을 가진 노드를 AZ내부에 생성하고 DNS로 접근
   - 주로 VPC 내부에 레이어 단위로 트래픽을 배분할 때 활용

-----
### ELB의 Node
-----
1. ELB가 생성 될 경우 각 AZ 및 서브넷에 Public 또는 Private IP를 가진 Node 생성 : 실제 요청은 이 Node를 통해 대상으로 전달
2. 이후 트래픽 요청사항 필요에 따라 최대 ALB 기준 100개까지 증설 (혹은 제거)
   - Scale 기준
     + ALB : 트래픽, Bandwidth, 동시 연결 숫자 등 + WAF 혹은 Lambda 등의 처리를 위한 연산 (일반적으로 5분에 2배 증가 가능 (예) 5Gbps 사용 시 5분 후 10Gbps까지 증설))
     + NLB : Bandwidth (분당 3Gbps 씩 증가 가능)

3. ALB의 경우 각 서브넷에 최소 하나(= 기본 두 개 서브넷 = 기본 두개) 생성
4. NLB의 경우 각 AZ별 하나씩 생성
<div align="center">
<img src="https://github.com/user-attachments/assets/6ac3d4c4-0b4b-416d-861a-5fc7d3d8b33a" />
</div>

5. DNS 요청이 들어오면 만들어진 Node의 IP 목록 전달 : 증감된 IP를 반영할 수 있도록 DNS의 TTL은 1분 권장
<div align="center">
<img src="https://github.com/user-attachments/assets/34815fe5-087b-4599-9470-a28389bea856" />
</div>

6. AWS 권장 사항 : 각 서브넷에 최소 8개 이상 IP 확보 (즉, /27 이상 권장)
<div align="center">
<img src="https://github.com/user-attachments/assets/6499fc00-80d3-4a86-ba7e-b2bda7160ba4" />
</div>
