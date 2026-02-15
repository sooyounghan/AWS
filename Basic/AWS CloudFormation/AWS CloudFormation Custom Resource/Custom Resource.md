-----
### Custom Resource
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/ca5ad708-3423-468c-99d8-57915f00c063" />
</div>

1. AWS 리소스 이외의 프로비전 로직을 지원하는 리소스 : CloudFormation에서 지원하지 않는 리소스 타입 프로비전 혹은 AWS와 무관한 독립적 로직 수행 가능
2. 기본적인 리소스처럼 생성 / 업데이트 / 삭제 지원
3. 용어
   - Custom Resource Provider : 커스텀 리소스를 프로비전하거나 로직을 수행하는 주체 (AWS 서비스(예) Lambda, EC2) 혹은 AWS 외부 주체도 가능 (사람, On-Premise 서버 등))
   - Template Developer : 커스텀 리소스가 포함된 템플릿을 작성하는 사람

4. 활용
   - Custom Resource Provider가 주어진 요청에 따라 수행할 로직 및 리소스 정의
   - Custom Resource Provider가 해당 로직 수행 주체에게 요청을 보낼 수 있는 SNS 토픽 혹은 Lambda 생성
   - Template Developer가 탬플릿 안에 커스텀 리소스 정의
     + 이 때, 전달할 Input과 SNS ARN 혹은 Lambda ARN을 같이 명시
   - 이후 해당 탬플릿을 프로비전(생성 / 업데이트 / 삭제) 할 때마다, 해당 커스텀 리소스에 SNS / Lambda로 요청을 보내고 응답 대기
   - 해당 리소스가 로직 처리를 완료하면 탬플릿에 응답
   - 성공이라면 이후 로직으로 넘어가며 실패(타임아웃 / 실패 응답 등) 한다면 실패 처리
<div align="center">
<img src="https://github.com/user-attachments/assets/264efcea-9b7e-4ddc-b977-04505a2bfa6b" />
<img src="https://github.com/user-attachments/assets/e27da354-a2be-4801-8431-bac547f60d3d" />
</div>

5. Demo - S3 버킷 비우기
   - S3의 경우 버킷에 파일이 있으면 삭제 불가 : CloudFormation 스택 삭제 시 버킷을 삭제할 때, 문제 발생 가능 (에러)
   - 커스텀 리소스로 미리 S3 버킷의 파일을 정리한 후 삭제를 진행하도록 수정
     + AWS Lambda를 활용한 커스텀 로직 수행
     + 삭제 트리거에만 반응하도록 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/beec1d86-1a4b-4f99-9386-ffa9b94cec72" />
<img src="https://github.com/user-attachments/assets/d94d1cf1-d2c1-46e0-9d93-45e3384a76bb" />
</div>

6. 주의사항
   - 기본 커스텀 리소스의 Timeout은 한시간
     + 즉, 잘못 프로비전하면 한 시간 동안 스택 삭제 불가능
     + 기본 타임아웃 설정 권장 (ServiceTimeout: 60 property)
   - Lambda로 커스텀 리소스를 구현할 경우, 삭제 시에도 수행
     + 즉, 첫 프로비전에 오류가 있다면 역시 불가능
