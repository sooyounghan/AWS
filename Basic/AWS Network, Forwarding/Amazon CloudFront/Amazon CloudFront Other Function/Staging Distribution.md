-----
### Staging Distribution
-----
1. CloudFront의 업데이트 전 Staging Distribution을 만들어 테스트 / 카나리 적용 가능
2. 워크 플로우
   - 원본 Distribution에서 파생된 Staging Distribution 새엉
   - Staging Distribution으로 Header / Weight 기반으로 트래픽 라우팅
   - 검수가 완료되면 Staging Distribution을 원본으로 승격

3. Staging Distribution은 원본과 다른 설정 가능 (중간 변경 가능)
   - Cache 정책 / Origin 설정, 에러 설정, 지역별 제한 등
   - S3의 경우 OAC 설정 별도로 추가 필요

4. Staging Distribution과 원본은 별도로 캐시 관리
5. CloudFront 서비스에 로드가 많을 경우 모든 요청을 Primary로 보내는 경우 발생 가능 : Distribution이 아닌 CloudFront 서비스 전체 단위
<div align="center">
<img src="https://github.com/user-attachments/assets/80c864e4-ea28-4e70-882c-f08771c5b361" />
<img src="https://github.com/user-attachments/assets/063e2459-915f-44c1-bd1a-d8a0fe072478" />
</div>
