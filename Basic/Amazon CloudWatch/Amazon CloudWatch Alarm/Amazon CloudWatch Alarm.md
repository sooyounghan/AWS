-----
### Amazon CloudWatch 경보
-----
1. 수집된 지표 값의 변동에 따라 발생하는 알림 생성 : 일정 수치(Threshold)로 도달하거나 이상 / 이하일 때 이벤트 발생
2. 3가지 상태
   - OK : 정상 상태
   - Alarm : 경보 상태
   - INSUFFICIENT_DATA : 경보 상태를 확인하기 위한 정보가 부족
3. 다양한 방법으로 대응 가능
   - SNS로 Lambda 실행, 이메일 전달 등
   - 예) 웹 서버의 500 에러가 일정 수치 이상일 때 슬랙 알림
4. 지표의 Resolution에 따라 경보 평가 주기 변동
   - High Resoultion이라면 60초 미만 주기 평가 가능
   - 이 외에는 모두 60초 배수 단위로 평가
<div align="center">
<img src="https://github.com/user-attachments/assets/0eb321c1-b8b9-4c55-a8c4-df81f10b12ed" />
<img src="https://github.com/user-attachments/assets/104fae65-c5c7-4485-b6f6-bea4b84bc4ea" />
</div>

5. 결합 경보(Composite Alarm)
   - 여러 정보를 결합한 경보 : Boolean (참 / 거짓) 관계로 경보를 묶어서 경보 조건 설정 (ANO, OR, NOT)
   - 많은 경보의 결과를 효율적으로 전달받기 위해 사용
   - 예) CPU 사용량 AND 네트워크 사용량 경보 = 웹 팀에 알림
   - 예) NOT 웹서버 CPU 사용량 AND RDS CPU 사용량 경보 = DB 팀에 알림
   - 예) (500 에러 경보 OR 400 에러 경보) AND 네트워크 사용량 경보 = 유저 피크 알림 전달
   - Suppressor Alarm 설정 가능 : 이 경보가 Alarm 상태 일 경우 Composite 경보 알림 중지
