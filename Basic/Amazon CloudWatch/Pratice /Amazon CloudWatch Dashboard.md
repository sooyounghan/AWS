-----
###  Amazon CloudWatch Dashboard 실습
-----
1. 순서
   - CloudWatch Dashboard에 AutoScaling Group의 다양한 지표 표시
   - Custom Dashboard 로직을 통해 직접 인스턴스 시작 / 재부팅 / 종료 및 AutoScaling의 Capacity 변경
   - 만든 대시 보드 공유

2. 알아둘 점
   - CloudFormation을 통해 예제 프로비전 예정 (CloudFormation : 코드로 인프라를 관리하는 IaaC (Infrastrcuture as a Code) 서비스
   - Lambda를 활용한 커스텀 로직 처리 (Lambda : AWS의 Serverless 컴퓨팅 서비스로, 간단한 로직을 서버 없이 처리)
<div align="center">
<img src="https://github.com/user-attachments/assets/fc756bc8-3d27-4f60-a140-e2e1a31cb4d7" />
</div>

3. CloudWach
   - 대시보드 - 대시보드 생성 : test-dashboard / 위젯 유형으로 변경 가능

4. AutoScaling Group
   - 시작 템플릿 생성 - demo-cw-template / t2.micro / 키 페어 사용하지 않음 / 보안 그룹 default
   - AutoScaling - demo-cw-asg / 시작 템플릿 : demo-cw-template / 가용 영역 모두 선택 / 최대 용량 2 / 태그 : Name - Demo-Cw-Asg
  
5. CloudFormation
   - 스택 생성 - 템플릿 파일 업로드 - cloudformation_template.txt 파일 업로드 - demo-cloudwatch-dashboard / 파라미터 : demo-cw-asg

6. CloudWatch 대시 보드 확인
   - 람다 함수 간접 호출 선택
   - EC2 AutoScaling 그룹의 그룹 지표 수집 활성화 필요 (해당 AutoScaling Group - 모니터링)
   - Max Size Increase, Desired Capacity
   - EC2 인스턴스 Terminate
   - 대시보드 공유 : 작업 - 대시보드 공유 - 공개적으로 공유 - 링크 복사
```
User: arn:aws:sts::362454057659:assumed-role/CWDBSharing-PublicReadOnlyAccess-V1QCKDIO/CognitoIdentityCredentials is not authorized to perform: lambda:InvokeFunction on resource: arn:aws:lambda:ap-northeast-2:362454057659:function:demo-cloudwatch-dashboard because no identity-based policy allows the lambda:InvokeFunction action
```
   - 대시보드에서 람다 함수를 부를 권한이 없으므로 문제 발생
     + 대시보드 공유 - IAM 역할 - 권한 정책 수정(편집)
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
            "lambda:InvokeFunction", // 추가
				"ec2:DescribeTags",
				"cloudwatch:GetMetricData"
			],
			"Resource": "*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"cloudwatch:GetInsightRuleReport",
				"cloudwatch:DescribeAlarms",
				"cloudwatch:GetDashboard"
			],
			"Resource": [
				"arn:aws:cloudwatch::362454057659:dashboard/demo-cw-asg-Dashboard",
				"arn:aws:cloudwatch:ap-northeast-2:362454057659:alarm:demo-cw-asg-HighCPUAlarm"
			]
		}
	]
}
```

   - 대시보드 공유 : 대시보드를 공유하고 사용자 이름 및 암호 요구
     + 임시 패스워드 : 메일로 전송 후 변경
     + 권한 조절 필요 (동일)

7. 리소스 정리
   - Auto-Scaling 그룹 / CloudFormation 에서 삭제하면 리소스 모두 삭
