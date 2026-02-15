-----
### Connection Pool
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/5764664f-95ad-4e33-ac7f-ea20aed7aa9c" />
<img src="https://github.com/user-attachments/assets/cd839c6c-8534-4a58-b6ac-466c64d79192" />
<img src="https://github.com/user-attachments/assets/98439ca0-3570-47e9-bbea-2fa4a34f4446" />
</div>

-----
### RDS Proxy
-----
1. AWS에서 제공하는 RDS의 Connection Pool 서비스
   - Connection Pool : 데이터베이스와 연결을 관리하고 재사용하여 애플리케이션 성능을 향상시키는 기능
   - RDS로 연결하는 Connection들을 중간에서 관리하여 효율적으로 배분 및 관리

2. AWS의 EC2 혹은 Lambda와 같이 대규모의 Connection이 필요한 경우 활용 가능
3. User / Password 인증 또는 IAM 인증 지원
4. 주의 사항
   - VPC 내부에서만 활용 가능 : Public Internet에서 접근 불가 (데이터베이스 접근 가능 여부와 무관)
   - 별도의 Endpoint 보유 : RDS와 다른 Endpoint 사용 필요 (추가로, Endpoint 생성 가능)
   - 기타 DB 엔진별 Port / 제약사항

5. RDS Proxy와 Secrets Manager
   - RDS Proxy는 Secrets Manager를 활용해 RDS에 접근
     + 즉, Secrets Manager에 RDS 자격증명 보관 필요
     + 각 유저별 다른 Secret 생성 필요

   - IAM DB 인증과 무관 : 즉, RDS Proxy는 IAM DB 인증을 사용하더라도, RDS Proxy에서 RDS는 Secrets Manager 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/4cd0391d-eb0d-4a90-9d23-6782ef63fbc2" />
<img src="https://github.com/user-attachments/assets/7ea70a4c-60b7-49cd-bee2-af9a3b022610" />
</div>

