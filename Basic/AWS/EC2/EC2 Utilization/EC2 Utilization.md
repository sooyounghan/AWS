-----
### 클라우드 환경의 EC2 활용 방식
-----
1. 수직 확장 (Vertical Scale : Scale-Up)
<div align="center">
<img src="https://github.com/user-attachments/assets/f62cbca6-3098-4b40-8a75-623c6c2c70b9" />
</div>

2. 수평 확장 (Horizontal Scale : Scale-Out)
<div align="center">
<img src="https://github.com/user-attachments/assets/40edaa59-5d5f-41a0-b857-8acd6fcc813d" />
</div>

3. 탄력성(Elastic) : 수요에 따라 컴퓨팅 파워 / 용량을 확장하거나 축소할 수 있는 능력
<div align="center">
<img src="https://github.com/user-attachments/assets/d7076e73-4572-4d8b-826e-c668979ec6eb" />
</div>

4. 클라우드 환경에서 컴퓨팅 자원 활용
   - On-Premise 환경 : 각 서버를 반영구적으로 사용
   - 클라우드 환경 : 각 인스턴스는 소모품으로 예고없이 장애가 발생하거나 통제된 방법으로 종료되는 것이 정상 (즉, '언제나 인스턴스는 예고없이 종료된다'라고 가정하고 아키텍쳐 설계 필요)
     + 필요하면 더 가져다 쓰고, 필요 없으면 버릴 수 있어야 함
   - Stateless하며, 고가용성 확보 필요

5. 고가용성과 Stateless
   - 고가용성 : 인스턴스 중 하나가 떨어져도 자동 복구 가능
   - Stateless : 인스턴스가 상태에 의존적이지 않음
     + 어떤 인스턴스도 특정 정보를 특별하게 저장하지 않음 = 언제나 추가될 수 있고, 삭제되어도 무방

6. 잘 활용하는 방법 : 언제나 떨어질 수 있으므로,
   - 인스턴스를 자동으로 Provision 할 수 있는 방법
   - 인스턴스를 가용영역 별로 분산할 수 있는 방법
   - 인스턴스 클러스터에 트래픽을 분산할 수 있는 방법
   - 인스턴스 각자 상태를 저장하지 않고, 언제나 삭제되고 추가되도 무방할 수 있는 방법 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/5f2d0f7d-588c-4d22-b82f-82fcfc84c91a" />
</div>

