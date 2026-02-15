-----
### 다른 스택의 Output 참조
-----
1. 다른 스택에서 Output 섹션으로 내보낸 값 참조 가능 : !ImportValue Intrisic 함수로 참조
2. 예) !ImportValue MyExportedName
3. Demo
```yml
Parameters:
  LatestLinuxAmiId:
    Type: "AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>"
    Default: "/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64"
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
      Name: !Sub "${AWS::StackName}-InstanceId" # Export
  EIP:
    Description: "The Elastic IP address of the EC2 instance"
    Value: !GetAtt MyEIP.PublicIp
    Export:
      Name: !Sub "${AWS::StackName}-EIP"

```
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : instance.yml - demo-ec2
   - 출력 - InstanceId 키와 해당 값 존재
     + 내보내기 이름 : demo-ec2-instance-id
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : reference_export.yml - demo-reference
     + 파라미터 : ExportInstanceIdName (demo-ec2-instance-id)
     + 서브넷 모두 선택
     + VpcId 선택
     + 프로비전 시 demo-ec2-instance-id의 InstanceID 참조
     + 로드밸런서로 DNS 접속 : Export 했던 InstanceID 출력
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
  
  ExportInstanceIdName:
    Type: "String"
    Description: "Name of that EC2 Instance id exported by another template"

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
        - Id: !ImportValue
            Fn::Sub: "${ExportInstanceIdName}"
      
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

   - 리소스 정리 : 스택 삭제
