-----
### 추가 파라미터 타입
-----
1. 일반적인 파라미터 타입(Integer / String) 이외에 CloudFormation에서 지원하는 추가 파라미터 타입
2. AWS Parameter : AWS 리소스를 명시하는 파라미터 타입
   - 예) ```AWS::EC2::Instance::Id```, ```AWS::EC2::Image::Id```, ```List<AWS::EC2::AvailabilityZone::Name>```
3. Systems Manager Parameter : AWS의 Systems Manager Parameter Store에서 값을 가져오는 타입
   - 예) ```AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>```
   - AWS에서 인프라를 보고 제어하기 위해 사용하는 AWS 서비스
   - Systems Manager 콘솔을 사용해 여러 AWS 서비스의 운영 데이터를 보고 AWS 리소스에서 운영 태스크 자동화할 수 있음
   - AWS SSM Parameter Store : AWS에서 주요 설정과 값들을 저장 / 관리 / 활용하기 위한 서비스 (예) API 주소, DB 호스트명, AMI ID, API Token, 유저 아이디 / 패스워드, 환경변수 등)
   - Public Parameter : AWS에서 공식적으로 배포하는 파라미터
     + 각 OS별 최신 AMI 아이디
     + AWS의 모든 리전 목록
     + AWS의 모든 서비스 목록
     + 특정 서비스를 사용 가능한 리전 목록

4. Parameter Store 값 참조
   - CloudFomration 템플릿 안에서 SSM Parameter Store의 값 참조 가능 : !Sub "{{resolve:ssm:파라미터 이름}}" 형식으로 사용
   - 주요 사용 사례
     + Public Parameter 참조 (AMI Image ID, Endpoint 등)
     + 환경 설정 값의 공유 (예) DB 패스워드, DB 호스트명 등)
     + 기타 주요 값들 설정
<div align="center">
<img src="https://github.com/user-attachments/assets/177332e4-6d6b-4b40-b5d3-2f5ad8de34d0" />
<img src="https://github.com/user-attachments/assets/230af7ed-fa53-40ce-9aa3-2b5397f18e7b" />
<img src="https://github.com/user-attachments/assets/f4e0aadb-44b0-4269-a8ad-e0d15e2a0565" />
<img src="https://github.com/user-attachments/assets/15c0a1c2-9f86-4920-a378-cf2fb16b3b0b" />
</div>

5. Demo
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : intanace_and_alb.yml / demo-instance-alb
     + LatestLinuxAmiId : Systems Manager - 파라미터 스토어 - 공용 파라미터 - ami-amazon-linux-latest - 값 (서울 리전의 Amazon Linux의 최신 AMI 값)
     + VpcId : 현재 VPC 목록 선택 가능
     + SubnetIds : Subnet ID를 리스트 타입으로 선택 
```yml
AWSTemplateFormatVersion: "2010-09-09"
Description: CloudFormation Template to Provision ALB, Target Group, and Two EC2 Instances

Parameters:
  LatestLinuxAmiId:
    Type: "AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>" # EC2 Image ID를 담은 파라미터 타입
    Default: "/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64"
  InstanceType:
    Description: "EC2 instance type."
    Type: "String"
    Default: "t3.micro"
    AllowedValues: ["t3.micro", "t3.small", "t3.medium", "t2.micro"]

  VpcId:
    Type: "AWS::EC2::VPC::Id"
    Description: "VPC ID where the resources will be created"

  SubnetIds:
    Type: "List<AWS::EC2::Subnet::Id>" # 리스트 타입 지정
    Description: "List of Subnet IDs for the ALB and EC2 instances"

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

  MyInstance:
    Type: AWS::EC2::Instance
    CreationPolicy:
      ResourceSignal:
        Timeout: PT15M
        Count: 1
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance"
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
  TargetGroup:
    Type: "AWS::ElasticLoadBalancingV2::TargetGroup"
    Properties:
      Name: "MyTargetGroup"
      Port: 80
      Protocol: HTTP
      VpcId: !Ref VpcId # 타입 지정
      TargetType: "instance"
      HealthCheckEnabled: true
      HealthCheckPath: "/"
      HealthCheckPort: "80"
      HealthCheckProtocol: HTTP
      Targets:
        - Id: !Ref MyInstance

  LoadBalancer:
    Type: "AWS::ElasticLoadBalancingV2::LoadBalancer"
    Properties:
      Name: "MyApplicationLoadBalancer"
      Subnets: !Ref SubnetIds
      Scheme: internet-facing
      LoadBalancerAttributes:
        - Key: idle_timeout.timeout_seconds
          Value: "60"
      SecurityGroups: [] # Add Security Group here

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
