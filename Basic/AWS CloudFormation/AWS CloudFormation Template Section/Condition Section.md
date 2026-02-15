-----
### Condition 섹션
-----
1. 조건에 따라 리소스 생성의 의사결정 가능
   - 예) Dev 리소스라면, 더 작은 타입의 EC2 인스턴스
   - 예) Prod 라면, Multi-AZ로 DB 구성
2. !If Intrinsic Function으로 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/5ec316e1-f833-4c89-822d-7d0839b0fb1b" />
</div>
