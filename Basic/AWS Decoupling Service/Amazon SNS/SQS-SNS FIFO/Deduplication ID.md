-----
### SQS FIFO - Deduplication ID
-----
1. DeduplicationID : SQS에서 각 메세지의 중복 여부를 판단하기 위한 고유 토큰
2. 메세지를 SQS에 전달할 때 부여할 수 있으며, 한번 특정 Deduplication ID를 가진 메세지가 처리된 경우 다음 5분 동안 같은 Deduplication ID를 가진 메세지는 무시
   - 성공은 정상적으로 반환, 메세지만 무시
   - 메세지가 전달된 이후에도 계속 레코드를 추적
3. 두 가지 방법으로 제공
   - Contents Based : SQS에서 자동적으로 메세지의 Body의 SHA-256 해시를 Deduplication ID로 사용 (💡 즉, Attribute는 HASH 과정에 포함하지 않음)
   - 메세지의 프로듀서가 직접 Duplication ID를 생성해서 같이 전달 (예) Timestamp 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/9e0cf8dd-37bf-4aa9-8f6e-37d173d28516" />
</div>
 
