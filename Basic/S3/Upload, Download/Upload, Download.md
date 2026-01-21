-----
### S3의 업로드, 다운로드 방법
-----
1. AWS Web Console
2. AWS SDK / AWS CLI
3. 일반 URL (버킷 / 파일이 공개되어있을 경우, 다운로드만 가능)
4. 미리 서명된 URL (Presigned URL)

-----
### 파일 공유하는 방법
-----
1. 영상 공유 서비스
2. 인증 정보 : IAM 유저, Access Key ID / Secret Access Key
<div align="center">
<img src="https://github.com/user-attachments/assets/ec7975f7-6bd3-4ad9-9b34-c872f6662fa1" />
</div>

-----
### 수동 파일 공유 문제점
-----
1. IAM 유저 개수 제한
2. 관리가 어려움
3. 만료 기간 설정이 어려움
4. 유출 시 모두에게 다시 공유 필요
5. 세세한 권한 조절 불가능
<div align="center">
<img src="https://github.com/user-attachments/assets/1e5aa7b8-eb70-4902-ab3a-4bdfe1866146" />
<img src="https://github.com/user-attachments/assets/38a3f6c0-7190-4025-92e8-1d7e5d0b5913" />
<img src="https://github.com/user-attachments/assets/4f8521d8-c4e4-4622-901a-88bf5f74884d" />
</div>

-----
### 미리 서명된 URL (Presigned URL)
-----
1. S3 파일을 안전하게 공유하고 싶을 때 사용
2. 생성자가 가진 권한으로 파일에 접근 가능한 임시 URL을 생성
   - URL의 만료 기간 지정 가능
   - Method(GET, PUT 등) 설정 가능
3. URL의 권한은 생성자가 가진 권한 중 일부 혹은 전체 사용 (예) 생성자가 GET 권한이 없다면 URL로 GET 불가능)
<div align="center">
<img src="https://github.com/user-attachments/assets/f5032554-e23f-4020-b3a5-06f0f9ca9f75" />
</div>

-----
### Amazon S3 멀티파일 업로드
-----
1. 하나의 파일을 여러 파트로 나누어 업로드 하는 방식
2. 장점
   - 빠른 속도
   - 파일 업로드의 컨트롤 보장 (예) 중간부터 업로드 하기)
3. 단점
   - 업로드 로직이 단일 파트보다 조금 더 복잡
   - 업로드에서 발생하는 파일들의 관리 포인트가 증가
4. AWS에서는 100 MB 이상 파일을 업로드할 경우 멀티파트 업로드 권장
<img width="1342" height="643" alt="image" src="https://github.com/user-attachments/assets/d9204ffe-aa4c-4b84-8c59-c08ca737147a" />

-----
### 주의사항
-----
1. 멀티파트 업로드가 중지되거나(명시적 중지가 아닌 에러 등), 업로드 이후 합치지 않을 경우, 업로드 된 파트 파일은 S3에 잔류
2. 비용이 발생
   - S3 Storge Lens 등의 서비스로 현재 완료되지 않은 멀티파트 업로드 파일 확인 가능
   - S3 생명 주기 설정을 통해 일정 시간 이후 삭제 가능 (예) 완료되지 않은 멀티파트 업로드의 경우 7일 후 삭제)
