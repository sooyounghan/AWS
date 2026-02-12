-----
### 지리적 배포 제한 (Geo Restriction)
-----
1. CloudFront 지리 배포 제한
   - Whitelist 혹은 Blacklist : 나라별 기준
   - 모든 배포(Distribution)에 제한 사항 포함 (즉, 일부만 제한 걸기 불가능)
   - IP 주소의 정확도는 99.8%
<div align="center">
<img src="https://github.com/user-attachments/assets/30fb35f1-6b58-4923-9096-384acd3c26a6" />
</div>

2. 3rd-Party 지리적 위치 서비스 사용
   - 커스텀 마이징 가능 (예) 브라우저 별, 쿠키 별 등)
   - Signed URL 기반
<div align="center">
<img src="https://github.com/user-attachments/assets/d2ba5c25-967f-42af-a90d-9a46bd371689" />
</div>

