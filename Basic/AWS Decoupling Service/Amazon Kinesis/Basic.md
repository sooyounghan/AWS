-----
### Kinesis Data Stream
-----
1. 스트리밍 데이터를 수집하고 처리 주체에 전달하는 서비스
   - 요청이 아닌 데이터의 처리
   - 단발성 데이터가 아니라 많은 분량으로 지속적으로 전달되는 데이터의 처리
   - 예) 로그, 이용 데이터, 센서 데이터

2. 작은 데이터부터 거의 무한대 용량을 처리 가능
3. 고가용성을 내부적으로 확보
4. 주요 구성 요소
   - Data Stream : 샤드(Shard)로 구성된 데이터 처리를 위한 스트림
     + Shards : Kinesis의 Data Stream의 처리 단위로 일종의 내부 파이프라인
   - 데이터 레코드 : Kinesis를 활용해 보내려하는 데이터
     + Sequence Number : 샤드 내의 파티션 키 단위로 부여되는 고유 번호
   - Producer : 데이터를 생산해서 Kinesis에 전달하는 주체
   - Consumer : 데이터를 소비하는 주체로 데이터를 직접 처리하거나 다른 주체로 전달
   - 액세스 정책 : Kinesis의 접근을 제어
<div align="center">
<img src="https://github.com/user-attachments/assets/38399c6d-2abd-47c3-ab88-6eb9e9f758b7" />
</div>

-----
### Data Stream
-----
1. 데이터 처리를 위한 스트림
2. 24시간 ~ 1년 단위로 데이터 저장 (SQS : 14일)
   - SQS와 달리 원한다면 이 기간 내 데이터를 다시 조회 / 처리 가능
3. 샤드 단위로 데이터 처리
4. Capacity Mode
   - On-Demand : 샤드를 수요에 따라 증설하고 사용한 데이터 용량 만큼 지불
   - Provisioned : 샤드 숫자를 지정하고 (변경 가능) 총 숫자 만큼 샤드에 대해서만 비용 지불

-----
### 샤드 (Shard)
-----
1. Kinesis의 Data Stream의 처리 단위로 일종의 내부 파이프라인
2. 정해진 용량의 데이터 처리
   - 읽기 : 5 Transactions / sec, 2MB / sec
   - 쓰기 : 1000 데이터 레코드 / sec, 1MB / sec
   - 즉, Data Stream의 처리 기능 용량은 보유한 샤드의 합으로 계산

3. 데이터 레코드의 Partition Key의 MD5 Hash 값을 기반으로 샤드 결정
4. 샤드 숫자 조절
   - Reshard : 샤드 숫자 조절 (기존 샤드를 2개로 Split / 2개를 합치는 Merge로만 가능), 즉, 기존 샤드를 분할 혹은 병합
<div align="center">
<img src="https://github.com/user-attachments/assets/9a191620-b011-49f0-b36c-c3ac2deecd3b" />
<img src="https://github.com/user-attachments/assets/c5efb8fc-0bb7-452a-96d7-7e5625ee43a3" />
<img src="https://github.com/user-attachments/assets/e0092bf3-241e-48e1-9639-30f6755ff888" />
</div>

   - Update Shard : Background에서 샤드 숫자를 지정한 숫자로 조절 (병합 / 분할)
   - OPEN / CLOSED / EXPIRED 상태로 관리

<div align="center">
<img src="https://github.com/user-attachments/assets/307bea06-8c86-4b90-a5a0-4d70d66abe88" />
<img src="https://github.com/user-attachments/assets/0d21793b-14f6-470b-89ff-dbc31cec08f9" />
<img src="https://github.com/user-attachments/assets/572581c3-a640-4884-b6e0-e062bf23d051" />
<img src="https://github.com/user-attachments/assets/c831493e-bad9-43fa-a2bc-a432bf27b1b3" />
</div>

-----
### Producer 
-----
1. 크게 3가지 방법으로 Kinesis 데이터 전송
2. API : PutRecord / PutRecords로 데이터 레코드 전달
3. Amazon Kinesis Producer Library (KPL) : Kinesis에 데이터 레코드를 전달하기 위한 라이브러리
   - Stream에 데이터를 전달하기 위한 다양한 기능 : Retry / 데이터 취합 및 Batch 전달 / CloudWatch 메트릭을 통한 모니터링
4. Amazon Kinesis Agent : JAVA 기반 애플리케이션

-----
### Consumer
-----
1. KCL (Amazon Kinesis Clienth Library) : Kinesis의 Data Record 읽기를 쉽게 관리하기 위해 만들어진 라이브러리
   - JAVA, Python, Ruby, Node.js, .NET 지원
   - 스트림 연결, 데이터 스트림 변경, 데이터 스트리밍의 로드 밸런싱, 리샤드 등 다양한 상황 및 로직 처리 기능 미리 구현
2. 직접 데이터를 처리하기도하고, 상황에 따라 다른 주체(S3, Openearch 등)에 전달

-----
### 기타
-----
1. KMS를 활용한 서버 사이드 암호화 지원
2. IAM 리소스 기반 정책으로 권한 부여 가능
3. CloudWatch에서 다양한 지표 수집
   - Basic : 기본 지표 수집 모드로 스트림 단위 지표를 1분 단위로 수집
   - Enhanced : 추가 비용으로 샤드 단위의 지표를 1분 단위로 수집
  
