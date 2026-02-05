-----
### Amazon SNS로 SMS 보내기
-----
1. 고려할 점
   - 특정 리전에서만 문자 메세지 보내기 가능 : 서울 리전은 사용 불가
   - Spending Threshold : SNS, SMS 최초 사용 시 설정되어 있는 지출 비용 제한 (기본 $1, 늘리려면 Support Case 오픈 필요)
   - Origination Identities 필요 : 나라마다 정책이 다르고 필수 여부 다름
     + Sender ID : 메세지를 보낸 대상으로 표시되는 ID
       * 예) US 리전은 Sender ID 사용 불가능 / India 리전에서는 필수
       * 몇몇 나라에서는 Sender ID가 필수이며, 몇 주의 기간이 소요되는 경우 발생
       * Support Case 등 지원 필요

     + Origination Numbers : 받은 사람에게 보이는 전달자 번호

2. Amazon SNS SMS Sandbox
   - 최초로 제공되는 환경으로 테스트 및 준비 진행
   - Sandbox에 있을 때는 지정된 번호로만 문제 메세지 가능
     + 최대 10개
     + 삭제는 24+ 시간 이후 번호 삭제 가능

   - Sandbox 탈출
     + 전화번호 인증
     + 테스트 문자 확인
     + Support Case에 연결 : 목적 / 사용 계획 / 리전 / Quota

3. SMS 메세지 종류
   - One Time Password : OTP 용도
   - Promotional : 홍보 용도
   - Transactional : Account 상태 알람, 배송 상태 확인 (마케팅 내용 포함 불가)
     
