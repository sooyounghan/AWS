-----
### 기타 기능
-----
1. Amazon CloudWatch Dashboard
   - AWS의 다양한 메트릭(지표) / 로그 데이터 기반으로 대시 보드 생성 가능 : 외부 커스텀 그래프 등 대시보드 커스터마이징 가능
   - 외부 공유 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/032d582e-816b-4fe3-8a61-9ffcfe6fd6b9" />
</div>

2. Amazon CloudWatch Synthesis
   - 일정 기반으로 구성 가능한 스크립트인 Canary를 수행해 엔드포인트 및 API를 모니터링 할 수 있는 서비스
     + Canary : Puppeteer 또는 Selenium Webdricver를 통해 헤드리스 Google Chrome 브라우저에 여러 액션 수행
       * Node.js 혹은 Python으로 코딩 가능
       * 1 ~ 60분 간격으로 수행 설정 가능
     + 주기적으로 실행되어 엔드포인트의 상태를 확인하고 에러 혹은 평소와 다른 내용이 있다면 기록
       * 시각 모니터링 가능 : 스크린샷 기반 비교 분석
       * HTTP 응답 코드 기반 분석 : HAR 파일 생성

   - 직접 지표를 생성하여 개시 : 다른 지표들처럼 대시보드 구성 / 알람 설정 가능

3. 인터넷 / 네트워크 모니터 : 전 세계 인터넷 날씨 지도 제공 (전 세계의 인터넷 장애 발생 상황과 영향을 받은 고객 위치 확인 가능)
<div align="center">
<img src="https://github.com/user-attachments/assets/9c43e679-5684-4e3f-a834-7f866f318fd7" />
</div>

   - 추가로 VPC, CloudFront, NLB 등 AWS 서비스와 관련된 내용을 별도 모니터링 가능
   - 네트워크 모니터로 온프레미스와 AWS 간 네트워크 모니터링 가능

4. CloudWatch Resource Health
   - Amazon EC2 현재 상태 정보를 모아 시각적으로 표현해주는 서비스 : 별도의 설정 없이 모든 EC2 메트릭, 태그 등 종합하여 실시간 시각화
   - CPU 사용률 / 메모리 사용률 / 상태 기준으로 색상을 입혀 인스턴스 표시 (예) CPU 0 ~ 10은 흰색, 10 ~ 50은 주황색, 50 ~ 100은 빨간색)
   - 시각화 커스터마이징 가능
     + 묶음 표시 기능 : Group By, Filter By 인스턴스 타입, 이미지 명, VPC ID 등
     + 인스턴스의 각 임계값 별 색상 커스터마이징 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/20abe197-231c-484b-904b-b9225e5ade08" />
</div>
