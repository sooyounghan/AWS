-----
### S3 액세스 로깅
-----
1. S3 버킷을 정해 활동 로그를 다른 S3 버킷에 저장하는 기능
   - 지정한 버킷에 로그 파일 저장
   - 반드시 기록 대상 버킷과 로그 저장 버킷 분리 필요 : 무한 파일 생성 방지
2. 보안 감사 및 분석 목적으로 활용

-----
### 이벤트 호출
-----
1. 객체의 생성 / 삭제 / 변경 이벤트 발생 : 다른 서비스 호출 (예) 파일 업로드 시 람다를 호출해 파일 무결성 검사)
2. SNS, SQS, Lambda 호출 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/1bc2e56f-0f17-4442-b6f5-750d4e803228" />
</div>

3. Demo
   - IAM - 역할 생성 : Lambda 선택 - AmazonS3FullAccess, CloudWatchFullAccessV2 - 이름 : demo-lambda-role-for-s3
   - Lambda - 함수 생성 - demo-image-resizer 및 Node.js 20.x 선택 - 기존 실행 역할 변경의 기존 역할 사용 : demo-lambda-role-for-s3 선택 후 함수 생성
   - 업로드 (.zip 파일) : demo-lambda-image-resizer 업로드
   - S3 생성 : demo-image-resize-{계정ID} - 속성 - 이벤트 알림 - 이벤트 알림 생성 (image-resize) / 접두사 : images/ - 객체 생성 : 모든 객체 생성 이벤트 - 람다 함수 지정 : 람다 함수에서 선택 (demo-image-resizer)
   - Lambda - 구성 - 트리거 확인
   - 폴더 만들기 : images - 이미지 파일 업로드 후 확인 (resized 디렉토리 내 리사이즈된 이미지 확인 가능)
   - 리소스 정리 : 버킷 삭제 및 람다 함수 삭제

-----
### 기타 기능
-----
1. 복제(Replication) : 지정 S3 버킷의 변경 내역을 다른 버킷으로 비동기로 복제하는 기능
   - 다른 리전, 다른 계정의 버킷으로도 복제 가능
   - Delete Marker 복제 가능 
   
2. Transfer Acceleration : CloudFront를 통해 다운로드를 최적화시키는 서비스
3. Request Pay : 버킷의 소유자 대신 요청자가 요청 및 다운로드 비용을 부담하는 모드
   - 주요 사용 사례 : 기업 간 데이터 공유, 리서치 데이터 공유 등
