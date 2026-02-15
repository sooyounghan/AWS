-----
### Amaazon SNS 메세지 구성 (Standard)
-----
1. Message Body : 실제 메세지 내용 (String)
   - 제목 / 💡 내용
   - 최대 256 KB (Message Attribute 포함)
   - 크기가 큰 컨텐츠의 경우 S3에 저장 후 버킷 / 키 정보만을 저장하는 형식으로 전달 가능
   - Raw Message Delivery : SNS 메세지 포맷을 따르지 않고, 전달받은 메세지 그대로 전달 (주로 S3 로깅, SQS 등 메세지를 파싱하지 않고, 있는 그대로 처리해야 할 경우 활용)
<div align="center">
<img src="https://github.com/user-attachments/assets/9a6cef1d-729d-401b-896c-d15331efaed8" />
</div>

2. Message Attribute : Key-Value 형식 메타데이터로 Body에 포함되지 않는 추가적인 데이터
   - 주로 분류 / 필터링 / Body를 처리하기 위한 Context 등에 활용 (예) Attribute에 영상 처리 알고리즘 명시 / 분류를 위한 Tag 정의 등)
   - Raw Message Delivery 활성화 시 최대 10개

3. TTL (모바일 전용)
4. 기타 : Timestamp, 토픽 ARN, 시그니처, 구독 해제를 위한 URL 등
