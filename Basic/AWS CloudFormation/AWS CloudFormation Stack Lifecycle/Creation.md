-----
### 스택의 생명 주기 - 생성
-----
1. 생성 : CloudFormation 템플릿을 기반으로 생성 (S3 경로를 지정 / 직접 템플릿 업로드(S3 업로드) 또는 Git에서 동기화)
2. 가능한 설정
   - IAM 역할 : 별도로 지정한 IAM 역할을 사용해서 프로비전 가능
   - 실패 동작
     + 모든 리소스 롤백 : 모든 리소스 프로비전 성공 또는 전체 롤백 (즉, 하나라도 실패하면 전부 롤백)
     + 성공한 리소스 보존 : 성공한 리소스는 그대로 두고 프로비전에 실패한 리소스만 롤백
   - 롤백 정책
     + 삭제 정책(DeletionPolicy) 활용 : DeletionPolicy 대로 삭제 여부 판단
     + 모든 리소스 삭제 : 롤백 중 생성된 리소스 무조건 삭제
   - 기타 : SNS 알림, 생성 제한 시간 등

3. Demo
   - CloudFront - 스택 생성 - 템플릿 파일 업로드 : should_fail.yml - demo-stackname
   - 스택 실패 옵션 선택 가능
   - 롤백 중 새로 생성된 리소스 삭제 선택 가능
```yml
#AP-Northeast-2 리전만 사용 가능
Resources:
  DeploymentBucket:
    Type: AWS::S3::Bucket
    DeletionPolicy: Delete
    Properties:
      BucketName: !Sub "mybucket-${AWS::AccountId}-${AWS::StackName}"

  #실패 예정 : 중복 버킷 이름
  DeploymentBucket2:
    Type: AWS::S3::Bucket
    DeletionPolicy: Delete
    Properties:
      BucketName: !Sub "mybucket-${AWS::AccountId}-${AWS::StackName}"
  #실패 예정 : 대문자 포함 버킷 이름
  DeploymentBucket3:
    Type: AWS::S3::Bucket
    DeletionPolicy: Delete
    Properties:
      BucketName: !Sub "Mybucket-${AWS::AccountId}-${AWS::StackName}"

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
```

   - CloudFront - 스택 생성 - 템플릿 파일 업로드 : should_fail.yml - demo-stack2
   - 스택 실패 옵션 선택 : 성공적으로 프로비저닝 된 리소스 보존 선택
   - 성공적으로 프로비저닝 된 리소스는 생성
