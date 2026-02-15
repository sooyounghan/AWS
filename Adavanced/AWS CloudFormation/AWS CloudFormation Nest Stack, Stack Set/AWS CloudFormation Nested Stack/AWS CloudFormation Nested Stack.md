-----
### Nested Stack
-----
1. 하나의 스택 안에 다른 스택을 포함하여 생성하는 기능 : 스택의 모듈화 가능
2. 계층 구조로 스택 프로비전 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/da8c9b61-6c3b-4b55-a23d-86e33fa33599" />
<img src="https://github.com/user-attachments/assets/fbf657d0-837f-435c-a597-b38004d9f854" />
</div>

3. Demo
```yml
Parameters:
  InstanceName:
    Type: "String"
    Default: "MyInstance"
  InstanceType:
    Description: "EC2 instance type."
    Type: "String"
    Default: "t3.micro"
    AllowedValues: ["t3.micro", "t3.small", "t3.medium", "t2.micro"]
Resources:
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
      ImageId: !Sub "{{resolve:ssm:/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64}}"
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
                `
              mode: "000755"
              owner: root
              group: root
          commands:
            00-install-agent:
              command: "./install_httpd.sh"
              cwd: "/home/ec2-user/"
            00-cfn-signal:
              command: !Join ["", ["/opt/aws/bin/cfn-signal -e 0 --stack ", !Ref "AWS::StackId", " --resource MyInstance --region ", !Ref "AWS::Region"]]

  SSHSecurityGroup:
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
Outputs:
  InstanceId:
    Description: "The Instance ID of the EC2 instance"
    Value: !Ref MyInstance
    Export:
      Name: !Sub "${AWS::StackName}-InstanceId"
  EIP:
    Description: "The Elastic IP address of the EC2 instance"
    Value: !GetAtt MyEIP.PublicIp
    Export:
      Name: !Sub "${AWS::StackName}-EIP"

```
   - Nested Stack
```yml
AWSTemplateFormatVersion: '2010-09-09'
Description: CloudFormation Template to Provision ALB, Target Group, and Two EC2 Instances

Parameters:
  InstanceType:
    Description: "EC2 instance type."
    Type: "String"
    Default: "t3.micro"
    AllowedValues: ["t3.micro", "t3.small", "t3.medium", "t2.micro"]
  
  VpcId:
    Type: "AWS::EC2::VPC::Id"
    Description: "VPC ID where the resources will be created"
  
  SubnetIds:
    Type: "List<AWS::EC2::Subnet::Id>"
    Description: "List of Subnet IDs for the ALB and EC2 instances"
  
  S3BucketName:
    Type: "String"
    Description: "S3 bucket name where the EC2 instance template is stored"
  
  TemplateFileName:
    Type: "String"
    Description: "EC2 instance template file name in the S3 bucket"

Resources:
  # 1. Target Group
  TargetGroup:
    Type: "AWS::ElasticLoadBalancingV2::TargetGroup"
    Properties:
      Name: "MyTargetGroup"
      Port: 80
      Protocol: HTTP
      VpcId: !Ref VpcId
      TargetType: "instance"
      HealthCheckEnabled: true
      HealthCheckPath: "/"
      HealthCheckPort: "80"
      HealthCheckProtocol: HTTP
      Targets:
        - Id: !GetAtt EC2InstanceStack1.Outputs.InstanceId
        - Id: !GetAtt EC2InstanceStack2.Outputs.InstanceId

  # 2. EC2 Instance 1 - Nested Stack
  EC2InstanceStack1:
    Type: "AWS::CloudFormation::Stack"
    Properties:
      TemplateURL: !Sub "https://s3.amazonaws.com/${S3BucketName}/${TemplateFileName}"
      Parameters:
        InstanceType: !Ref InstanceType
        InstanceName: "EC2-Instance-1"
      TimeoutInMinutes: 5

  # 3. EC2 Instance 2 - Nested Stack
  EC2InstanceStack2:
    Type: "AWS::CloudFormation::Stack"
    Properties:
      TemplateURL: !Sub "https://s3.amazonaws.com/${S3BucketName}/${TemplateFileName}"
      Parameters:
        InstanceType: !Ref InstanceType
        InstanceName: "EC2-Instance-2"
      TimeoutInMinutes: 5

  # 4. ALB
  LoadBalancer:
    Type: "AWS::ElasticLoadBalancingV2::LoadBalancer"
    Properties:
      Name: "MyApplicationLoadBalancer"
      Subnets: !Ref SubnetIds
      Scheme: internet-facing
      LoadBalancerAttributes:
        - Key: idle_timeout.timeout_seconds
          Value: "60"
      SecurityGroups: []  # Add Security Group here

  # 5. ALB Listener
  Listener:
    Type: "AWS::ElasticLoadBalancingV2::Listener"
    Properties:
      DefaultActions:
        - Type: forward
          TargetGroupArn: !Ref TargetGroup
      LoadBalancerArn: !Ref LoadBalancer
      Port: 80
      Protocol: HTTP

Outputs:
  LoadBalancerDNSName:
    Description: "DNS name of the ALB"
    Value: !GetAtt LoadBalancer.DNSName
```
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : instance.yml - S3 URL 복사
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : nested_stack.yml
     + demo-my-nested-stack
     + t3.micro
     + S3BucketName : instance.yml - S3에 해당하는 버킷 선택
     + 템플릿 파일 이름 : S3 URL 복사 뒤쪽
     + 기본 VPC 선택

   - 두 개의 CloudFormation 스택 생성
   - 스택 삭제 
     + 리소스 확인
     + 스택 확인 : 중첩 표시 보임 (뷰 중첩됨)
     + EC2 - 로드밸런서 확인 및 로드밸런서 DNS로 접속
     
