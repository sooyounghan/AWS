-----
### NoSQL
-----
1. NoSQL (Not Only SQL) : 스키마 없이 다양한 형식의 데이터를 처리하는 데이터베이스 시스템 (별도로 정해진 스키마가 없으며 유연하게 데이터 저장)
2. 종류
   - Key-Value Stores : Key-Value 쌍으로 데이터 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/51e1d17e-d90d-4ab7-bf5a-3a34cee1a57a" />
</div>

   - Document Database : 데이터를 JSON 형식과 비슷한 일종의 문서 형식으로 데이터를 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/3c1944a0-5e90-4c7b-90a2-d3dc5040ece1" />
</div>

   - Graph Database : 데이터 간 연결을 중심으로 데이터를 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/b8905bc1-b339-49b7-83c6-0023238fdcda" />
</div>

3. BASE 기반 처리
4. BASE : 일반적으로 ACID에 비해 훨씬 느슨하고 유연한 모델
   + BAsicly Available : 가용성을 중심으로 처리 (예) 가용성을 유지하기 위해 데이터는 다수 스토리지에 저장하며 접근 가능하게 유지)
   + Soft-State : 데이터베이스의 상태는 느슨하고 유연함. 즉, 확정된 상태로 계속 유지하지 않고 자연스럽게 원하는 상태로 수렴
   + Eventual Consistency : 일관성이 바로 확보되지 않고, 시간이 지나서 언젠가 일관적 상태로 수렴
  
