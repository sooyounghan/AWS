<div align="center">
<img src="https://github.com/user-attachments/assets/e9728d2e-d9ac-4aaf-a4cb-5c68aa90c0ed" />
</div>

-----
### 몇 가지 개선점
-----
1. 도메인 연결 (HTTPS)
2. 모니터링 대시보드
   - CPU 사용률
   - 디스크 사용률 등
3. CDN (CloudFront)
<div align="center">
<img src="https://github.com/user-attachments/assets/8234b54b-9008-4fde-80f6-c83d80051c52" />
<img src="https://github.com/user-attachments/assets/a8de57f5-983f-4cf6-b6a3-5f4c4f3835f8" />
</div>

-----
### Demo - 기존 아키텍쳐에서 HTTPS 추가
-----
1. CloudFormation으로 기존 아키텍쳐 되살리기 : WordPress Set-Up 전까지 실시
2. ALB에 HTTPS 리스너를 추가하여 HTTPS 구현
   - 사용자가 HTTP 접속 시, HTTPS로 Redirection
   - CloudFront 연동
3. CloudWatch 지표 기반 대시보드 생성하여 모니터링

-----
### 주의사항 : 인프라로 HTTPS를 구현할 경우
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/7654c916-2a60-4ea4-8dc8-b9039708ab51" />
<img src="https://github.com/user-attachments/assets/09a87a05-04ee-405d-9779-b868f7e12019" />
<img src="https://github.com/user-attachments/assets/bb8f99f6-f25f-4cde-b67a-cf3ed73fe6df" />
<img src="https://github.com/user-attachments/assets/08c50b5d-af04-469a-b947-3ce55373a23e" />
<img src="https://github.com/user-attachments/assets/e8ce9d1c-0951-4bb9-8e16-30c2cbc7584f" />
<img src="https://github.com/user-attachments/assets/25d26ca0-3d93-45f2-8d64-2d3e621578fd" />
</div>
