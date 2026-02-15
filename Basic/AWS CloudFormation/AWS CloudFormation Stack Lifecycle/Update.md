-----
### 스택의 생명 주기 - 업데이트
-----
1. 기존 템플릿 기반으로 파라미터만 변경 또는 새로운 템플릿 기반 업데이트 (S3 경로를 지정 / 직접 템플릿 업로드(S3 업로드) 또는 Git에서 동기화)
2. 가능한 설정 : 생성과 거의 동일
3. Stack Policy : 스택 리소스의 불필요한 업데이트를 방지하기 위한 일종의 보호 장치
   - 💡 설정 시, 명시적으로 허용한 리소스를 제외하고는 업데이트 불가능
   - 💡 한 번 설정 시 삭제 불가능
   - 주요 사용 사례 : 특정 리소스만 업데이트 허용 / 특정 리소스 삭제 방지 (스택 삭제는 가능)

4. 변경 세트로 변경 내용에 대해 미리 확인 가능 : 단, 변경 성공 여부를 보장하진 않음
<div align="center">
<img src="https://github.com/user-attachments/assets/793d9906-2f93-417c-b87d-3f1290af7e22" />
<img src="https://github.com/user-attachments/assets/2bccf81c-a7ce-4897-9265-5ee47b5f4faf" />
<img src="https://github.com/user-attachments/assets/368c8b9b-6f3a-4831-be1d-d3cea220bc47" />
</div>

5. Demo
```yml
#AP-Northeast-2 리전만 사용 가능
Resources:
  DeploymentBucket:
    Type: AWS::S3::Bucket
    DeletionPolicy: Delete
    Properties:
      BucketName: !Sub "mybucket-${AWS::AccountId}-${AWS::StackName}"

  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance"
      InstanceType: "t2.micro"
      ImageId: ami-0023481579962abd4
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
  MyInstance2:
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance2"
      InstanceType: "t2.micro"
      ImageId: ami-0023481579962abd4
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
```
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : original_resource.yml - demo-original-resource
     + 스택 정책 : 파일 업로드 - stack_policy_allow_ec2_only
```json
{
  "Statement": [
    {
      "Effect" : "Allow",
      "Principal" : "*",
      "Action" : "Update:*",
      "Resource" : "*",
      "Condition" : {
        "StringEquals" : {
          "ResourceType" : ["AWS::EC2::Instance"]
        }
      }
    }
  ]
}
```

   - demo-original-resource - 업데이트 - 기존 템플릿 교체 : updated_resource.yml (업데이트 전체 롤백 - ROLLBACK_COMPLETED)
```yml
#AP-Northeast-2 리전만 사용 가능
Resources:
  DeploymentBucket: # 업데이트 불가
    Type: AWS::S3::Bucket 
    DeletionPolicy: Delete
    Properties:
      BucketName: !Sub "mybucket-${AWS::AccountId}-${AWS::StackName}"
      CorsConfiguration:
        CorsRules:
          - AllowedHeaders:
              - "*"
            AllowedMethods:
              - "PUT"
            AllowedOrigins:
              - "*"
            Id: myCORSRuleId1

  MyInstance: # 업데이트 가능
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance"
      InstanceType: "t3.micro"
      ImageId: ami-0023481579962abd4
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
  MyInstance2:
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance2"
      InstanceType: "t3.nano"
      ImageId: ami-0023481579962abd4
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
```

   - demo-original-resource - 업데이트 - 기존 템플릿 교체 : updated_resource.yml - 스택 실패 옵션 : 성공적으로 프로비저닝된 리소스 보존
     + EC2 업데이트만 실시
     + 부분만 실행된 것이므로 롤백 / 업데이트 / 재시도 가능
     + 롤백 실시

   - 💡 MyInstance만 허용
     + 스택 정책에서 콘솔에서는 불가
     + CloudShell에서 가능 : 다음 명령어 부여 후 입력하면 Update 실시
```json
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "Update:*",
      "Principal": "*",
      "Resource": "LogicalResourceId/MyInstance"
    }
   
  ]
}
```
```
aws cloudformation set-stack-policy \
    --stack-name demo-original-resource \
    --stack-policy-body '{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "Update:*",
      "Principal": "*",
      "Resource": "LogicalResourceId/MyInstance"
    }
   
  ]
}'
```

   - demo-original-resource - 업데이트 - 기존 템플릿 교체 : updated_resource_instance_delete.yml - 스택 실패 옵션 : 성공적으로 프로비저닝된 리소스 보존
```yml
#AP-Northeast-2 리전만 사용 가능
Resources:
  DeploymentBucket:
    Type: AWS::S3::Bucket
    DeletionPolicy: Delete
    Properties:
      BucketName: !Sub "mybucket-${AWS::AccountId}-${AWS::StackName}"
      CorsConfiguration:
        CorsRules:
          - AllowedHeaders:
              - "*"
            AllowedMethods:
              - "PUT"
            AllowedOrigins:
              - "*"
            Id: myCORSRuleId1

  MyInstance: # MyInstance2 부분 삭제
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance"
      InstanceType: "t3.micro"
      ImageId: ami-0023481579962abd4
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
```

  - 업데이트 중 리소스 삭제 방지
```json
{
  "Statement": [  
    {
      "Effect" : "Allow",
      "NotAction" : "Update:Delete", # 업데이트 중 리소스 삭제 방지
      "Principal": "*",
      "Resource" : "*"
    } 
  ]
}
```
