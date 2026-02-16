-----
### Demo - 이미지 리사이저
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/fcde1d31-8db2-4f97-a250-a86974e73875" />
</div>

1. 이미지를 업로드하면 지정한 사이즈로 리사이징 후 다운로드 할 수 있는 웹 사이트
2. Backend
   - Node.js 기반 AWS의 2-Tier 아키텍쳐(ALB-ASG-EC2)로 호스팅
   - CI / CD 파이프라인으로 소스 업데이트 → 빌드 → 배포 과정 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/2404148e-5468-4baa-b3b9-1a7fdc374e1e" />
</div>

3. Frontend
   - Next.js 기반 Static 페이지로 S3로 호스팅
   - CI / CD 파이프라인 기반으로 소스 업데이트 → 빌드 → 배포 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/115ace33-763b-483c-ac46-a7e895ca4ae8" />
</div>

4. 소스 : Git 활용
