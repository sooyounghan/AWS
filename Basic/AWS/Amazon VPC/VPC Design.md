-----
### VPC 설계 고려사항
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/7d99a1ac-443d-44ba-9e87-c59651905258" />
</div>

1. VPC 간의 IP Range : VPC 간 Peering을 위해 서로 IP Range가 겹치지 않아야 함
2. 고가용성을 위한 서브넷 및 애플리케이션 티어
   - 고가용성 : 적어도 3개 이상의 가용 영역에 분산 필요 + 추가 가용 영역
   - 애플리케이션 티어 : 보통 3 Tier 아키텍쳐가 기본 + 추가 예비 티어
3. 서브넷 단위 : /20 또는 /24
   - /20 : 많은 IP 숫자
   - /24 : 알아보기 쉬움
<div align="center">
<img src="https://github.com/user-attachments/assets/5163c395-8e4e-47f5-811a-e2373ea81b53" />
</div>
