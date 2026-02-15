-----
### Demo
-----
1. Demo - AWS Public Parameter 조회 (서울 리전)
   - Systems Parameter - Parameter Store - 공용 파라미터 - 공용 파라미터 확인 가능
   - CloudShell에서 다음 명령어 입력
     + AWS 모든 리전 목록
     + AWS의 모든 서비스 목록
     + 특정 서비스를 사용 가능한 리전 목록
     + 그 외 조회
```
//특정 서비스 사용 가능한 리전 목록
aws ssm get-parameters-by-path \
  --path /aws/service/global-infrastructure/services/bedrock/regions --output json | \
  jq '.Parameters[].Value'


//특정 리전에서 사용 가능한 서비스 목록
aws ssm get-parameters-by-path \
  --path /aws/service/global-infrastructure/regions/us-east-1/services --output json | \
  jq '.Parameters[].Name' | sort | head -10

//리전 목록
aws ssm get-parameters-by-path \
  --path /aws/service/global-infrastructure/regions --output json | \
  jq '.Parameters[].Name'

//서비스 목록
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/services \
    --query 'Parameters[].Name | sort(@)'

//서비스명 조회
aws ssm get-parameters-by-path \
  --path /aws/service/global-infrastructure/services/rosa --output json | \
  jq '.Parameters[].Value'

//총 서비스 숫자 조회
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/services \
    --query 'length(Parameters[].Name)'

//리전 숫자
aws ssm get-parameters-by-path \
  --path /aws/service/global-infrastructure/regions  \
  --query 'length(Parameters[].Name)'
```

3. Demo - SSM Parameter 활용
   - CloudFormation - 템플릿 파일 업로드 : template.yml - demo-image-handler (ProjectName 동일)

   - 이미지를 처리하는 두 가지 방식 스위칭을 파라미터 스토어 활용
     + Systems Parameter - Parameter Store - 파라미터 생성
     + /demo-image-handler/current_sqs_url
     + 계층 : 표준
     + 유형 : 문자열
     + 값 : ```*.*```
   
   - 이미지 리사이징 / 로테이션
     + SQS
     + demo-image-handler-resize-request-queue - Queue URL 복사
       * Systems Parameter - Parameter Store - /demo-image-handler/current_sqs_url 편집 - 값에 : Queue URL 입력
       * S3 버킷 (demo-image-handler-filebucket-{계정ID} - images 폴더 생성 후, flower.jpg 업로드)
       * 이미지 리사이징 확인 (/resized 디렉토리 확인 후, flower.jpg 이미지 확인)
       * 정사각형 변경 : Systems Parameter - Parameter Store - dimension 파라미터 - 편집 - 값 : 200 x 200 변경 후 재실시

     + demo-image-handler-rotate-request-queue - Queue URL 복사
       * Systems Parameter - Parameter Store - /demo-image-handler/current_sqs_url 편집 - 값에 : Queue URL 입력
       * S3 버킷 (demo-image-handler-filebucket-{계정ID} - images 폴더 생성 후, flower.jpg 업로드)
       * 이미지 로테이션 확인 (/rotated 디렉토리 확인 후, flower.jpg 이미지 확인)

   - CloudFormation 스택 삭제 및 파라미터 삭제
<div align="center">
<img src="https://github.com/user-attachments/assets/1fc3f782-590c-4ebd-a0f8-71ae0c4e76d8" />
<img src="https://github.com/user-attachments/assets/b9eedbb2-0683-495e-b8c6-989010b55935" />
</div>

3. 기타 주의사항
<div align="center">
<img src="https://github.com/user-attachments/assets/7e4d62a0-f448-4055-98fc-717a9a261065" />
</div>

   - 초당 만 건 트랜잭션 지원
   - 큰 규모 대규모 아키텍쳐라면, 매번 조회 또는 막대한 트래픽의 경우에는 여유치 않을 수 있음
