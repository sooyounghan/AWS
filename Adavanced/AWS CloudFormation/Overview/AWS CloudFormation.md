-----
### AWS CloudFormation
-----
1. AWS 리소스를 모델링하고 설정하여 리소스 관리 시간을 줄이고, AWS에서 실행되는 애플리케이션에 더 많은 시간을 사용하도록 해주는 서비스
2. What : 인프라를 코드로 구성할 수 있는 무료 서비스(IaC)
3. When
   - 인프라를 자동으로 프로비전 하고 싶을 때
   - 인프라를 재사용하거나 공유하고 싶을 때
   - 정형화된 아키텍쳐를 코드화시켜 관리하고 싶을 때
   - 인프라의 프로비전 및 업데이트 과정을 관리하고 싶을 때

4. How
   - 코드 기반으로 AWS 인프라를 여러 리전에 자동으로 프로비전하거나 업데이트 : 프로비전 / 업데이트 과정에서 오류 발생 시 자동 롤백
   - JSON과 YAML을 지원하고 재사용성 및 편의를 위한 수많은 기능 포함
   - 커스텀 리소스를 사용해서 거의 모든 행동 가능 (슬랙 알림 / On-Prem 데이터 센터 리소스 구성 등)

-----
### Demo - CloudFormation으로 EC2 웹 서버 만들기
-----
1. CloudFormation 스택 생성
   - CloudFormation - 스택 생성 - 기존 템플릿 - 템플릿 파일 업로드(instance.yml) - demo-ec2-instance
   - 인스턴스 이름 : MyInstance
   - 인스턴스 타입 : t3.micro
   - AWS CloudFormation에서 IAM 리소스를 생성할 수 있음을 승인합니다. 활성화
   - instance.yml
```yml
Resources: ## 생성하고 싶은 리소스
  InstanceRole:
    Type: "AWS::IAM::Role"
    Properties:
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - ec2.amazonaws.com
            Action:
              - "sts:AssumeRole"
      Path: /
      ManagedPolicyArns: ["arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore", "arn:aws:iam::aws:policy/service-role/AmazonEC2RoleforSSM", "arn:aws:iam::aws:policy/AmazonS3FullAccess", "arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy"]
      Policies:
        - PolicyName: ssm-getparameter
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action: ["cloudformation:*", "codecommit:*", "codepipeline:*", "ssm:*", "secretsmanager:*", "execute-api:*", "lambda:*", "logs:*", "sqs:*", "config:*"]
                Resource: "*"
  InstanceProfile:
    Type: "AWS::IAM::InstanceProfile"
    Properties:
      Path: /
      Roles:
        - !Ref InstanceRole
  MyEIP:
    Type: AWS::EC2::EIP

    Properties:
      InstanceId: !Ref MyInstance
      Tags:
        - Key: "Name"
          Value: !Ref InstanceName
  MyInstance:
    Type: AWS::EC2::Instance
    CreationPolicy:
      ResourceSignal:
        Timeout: PT15M
        Count: 1
    Properties:
      Tags:
        - Key: "Name"
          Value: !Ref InstanceName
      InstanceType: !Ref InstanceType
      SecurityGroups:
        - !Ref SSHSecurityGroup
      AvailabilityZone:
        Fn::Select: ["0", Fn::GetAZs: !Ref "AWS::Region"]
      ImageId: !Ref LatestLinuxAmiId
      IamInstanceProfile: !Ref InstanceProfile
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash -xe
          # Get the latest CloudFormation package
          yum update -y aws-cfn-bootstrap
          # Start cfn-init
          /opt/aws/bin/cfn-init -s ${AWS::StackId} -r MyInstance --region ${AWS::Region}
    Metadata:
      AWS::CloudFormation::Init:
        config:
          files:
            /home/ec2-user/install_httpd.sh:
              content: |
                #!/bin/bash -xe            
                dnf install httpd -y
                service httpd start
                TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
                INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
                echo "$INSTANCE_ID" >> /var/www/html/index.html

              mode: "000755"
              owner: root
              group: root
          commands:
            00-install-agent:
              command: "./install_httpd.sh"
              cwd: "/home/ec2-user/"
            00-cfn-signal:
              command: !Join ["", ["/opt/aws/bin/cfn-signal -e 0 --stack ", !Ref "AWS::StackId", " --resource MyInstance --region ", !Ref "AWS::Region"]]

  SSHSecurityGroup: ## 보안그룹 
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable SSH access via port 22
      SecurityGroupIngress:
        - CidrIp: 0.0.0.0/0
          FromPort: 22
          IpProtocol: tcp
          ToPort: 22
        - CidrIp: 0.0.0.0/0
          FromPort: 80
          IpProtocol: tcp
          ToPort: 80
      Tags:
        - Key: Name
          Value: DemoEC2InstanceSecurityGroup
```
   - CREATE_COMPLETE : 완료

2. EC2 확인
   - MyInstance EC2 인스턴스 확인 
   - EC2 변경 : 스택 업데이트 - 직접 업데이트 - 기존 템플릿 사용 - 인스턴스 이름 변경 : MyEC2-Instance / 인스턴스 타입 : t3.small - 변경 사항 미리보기로 확인 가능 (UPDATE_IN_PROGRESS - UPDATE_COMPLETE)

3. 리소스 정리 : 스택 삭제
