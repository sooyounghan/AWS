-----
### Parameter Policy
-----
1. 파라미터에 지정할 수 있는 관리 정책
2. 세 가지 종류
   - Expiration : 일정 기간 이후 삭제 (TTL)
   - ExpirationNotification : 삭제 전 EventBridge 이벤트 생성
   - NoChangeNotification : 일정 기간 이상 파라미터 변경이 없을 경우 EventBridge 이벤트 생성
3. Advanced Tier만 가능
4. 삭제 / 갱신 가능
