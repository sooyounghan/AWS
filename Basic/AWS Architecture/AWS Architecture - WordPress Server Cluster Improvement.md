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
1. CloudFormation으로 기존 아키텍쳐 되살리기 : WordPress Set-Up 전까지 실시 (cloudformation.yml)
   - 서울 리전
   - CloudFormation - 스택 - 스택 생성 - 템플릿 업로드 : cloudformation.yml - demo-wordpress - wordpress.Route53 도메인
   - DB Passwrod : abcd1234
   
2. ALB에 HTTPS 리스너를 추가하여 HTTPS 구현
   - EC2 - 로드 밸런서 - 생성된 로드 밸런서 - 리스너 및 규칙 - 리스너 추가
     + 프로토콜 : HTTPS
     + 대상 그룹 : MyTG-demo-wordpress
     + 인증서 선택

   - Route 53 - 호스팅 영역 - 도메인 - 레코드 생성 - wordpress 레코드 이름 / 레코드 유형 : A / 서울 리전 / ALB 별칭 (별칭 활성화) / 로드밸런서 선택

   - 사용자가 HTTP 접속 시, HTTPS로 Redirection
     + 로드밸런서 - 리스너 및 규칙 - HTTP:80 리스너 규칙 관리 - 규칙 편집
     + HTTP 프로토콜 - URL 리다이렉션 - 프로토콜 : HTTPS / 포트 : 443 - 상태 코드 : 301 (영구 이동)

   - CloudFront 연동
     + 배포 생성 - 배포 오리진 - MyALB-demo-wordpress
     + 프로토콜 : HTTP
     + 뷰어 프로토콜 : Redirect HTTP to HTTPS
     + 캐시 정책 : CachingDisabled
     + 원본 요청 정책 : AllViewer
     + 대체 도메인 이름 : wordpress.Route53 DNS
     + 인증서 설정

   - MyALB-demo-wordpress
     + 로드밸런서 - 리스너 및 규칙 - HTTP:80 리스너 규칙 관리 - 규칙 편집
     + HTTP 프로토콜 - 대상 그룹으로 전달 : MyTG-demo-wordpress

   - Route 53 대신 CloudFront로 연결
     + 레코드 편집 - 트래픽 라우팅 대상 : CloudFront 배포에 대한 별칭
     + 배포 선택

   - ALB에서 CloudFront 우회하지 않고 접속 가능 방지
     + EC2 - 보안 그룹 - ALBSecurityGroup - 인바운드 규칙 - 편집
     + 삭제 후, 규칙 추가 : 모든 트래픽 / 소스 : global.cloudfront.origin-facing 선택 

4. CloudWatch 지표 기반 대시보드 생성하여 모니터링
   - EC2 - MyASG-demo-wordpress - 모니터링 - Autoscaling 그룹 지표 활성화
   - CloudWatch - 대시보드 - 대시보드 생성 - demo-dashboard-wordpress
     + MyASG-demo-wordpress 검색 (NetworkIn / Out / CPUUtilization / Max, Min Size)
   - 지표 설정 후 저장

5. 리소스 정리 : 대시보드 정리 / CloudFormtion 스택 삭제
   
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
