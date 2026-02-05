-----
### Amazon SQS Message
-----
1. SQS에서 전달하는 데이터 단위
2. 최대 사이즈 : 1 MB, 최대 14일까지 저장 가능
3. SQS 메세지는 총 3가지 상태
   - Stored : Producer가 메세지를 SQS Queue에 전달을 완료하여 대기 중인 상태 (최대 개수 제한 없음)
   - In Flight : Consumer가 메세지를 가져와서 처리중인 상태 (Standard 약 120,000개 / FIFO 약 20,000개)
   - Deleted : Consumer가 메세지 내용 처리 후 삭제한 상태
<div align="center">
<img src="https://github.com/user-attachments/assets/e2e857d1-72b0-41ba-8d89-7c505e2f4ca1" />
</div>

4. 메세지 구성
   - Message Body : 실제 메세지 내용 (String)
     + 최대 1MB (Message Attribute 포함)
     + 크기가 큰 컨텐츠의 경우 S3에 저장 후 버킷 / 키 정보만을 저장하는 형식으로 전달 가능

   - Message Attribute : Key-Value 형식의 메타데이터로 Body에 포함되지 않는 추가적인 데이터
     + 주로 분류, 필터링, Body를 처리하기 위한 Context 등에 활용 (예) Attribute에 영상 처리 알고리즘 명시 / 분류를 위한 Tag 정의 등)
     + 최대 10개

   - Message System Attribute : AWS 서비스에서 활용하기 위한 Metadata
     + 현재는 AWS X-Ray를 위한 AWSTraceHeader만 지원

   - ReceiptHandle : 메세지를 삭제하기 위한 키
   - 기타 정보 : Region, Event Source, MD5, Timestamp, 해시 등
   - FIFO의 경우 Message Group ID / Deduplication ID
<div align="center">
<img src="https://github.com/user-attachments/assets/8985c510-1b06-4d3c-8bc2-faedbffe4f96" />
<img src="https://github.com/user-attachments/assets/de982883-1daa-40bb-879c-c38e99749f53" />
</div>
