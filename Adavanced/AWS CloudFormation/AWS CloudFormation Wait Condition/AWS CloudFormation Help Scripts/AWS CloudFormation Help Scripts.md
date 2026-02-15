-----
### CloudFormation Help Scripts
-----
1. CloudFormation과 소통을 도와주기 위한 Python 기반 스크립트
2. cfn-signal : CloudFormation의 Wait Condition 리소스 (혹은 CreationPolicy가 붙은 리소스에 처리 완료 신호를 보내주는 스크립트)
<div align="center">
<img src="https://github.com/user-attachments/assets/19efb2a5-1c46-456c-a511-03d7d232d5eb" />
<img src="https://github.com/user-attachments/assets/5c11cf39-01f0-4454-b295-8b344ef82688" />
</div>

3. cfn-init : 리소스 메타데이터 기반으로 패키지 설치나 파일 생성 등 담당
<div align="center">
<img src="https://github.com/user-attachments/assets/9c08e824-5c1b-4a99-8e3e-ac0b375bfdeb" />
<img src="https://github.com/user-attachments/assets/56fa9877-6247-4cd7-ac26-62b1976091ce" />
</div>

4. cfn-get-metadata : 특정 경로의 메타데이터를 확보하는 스크립트
5. cfn-hup : 업데이트를 체크하여 변경이 일어나면 커스텀 로직을 수행하는 스크립트
<div align="center">
<img src="https://github.com/user-attachments/assets/06444170-4402-4702-9a6b-202b5d1a1dd6" />
<img src="https://github.com/user-attachments/assets/455c5987-e819-49ca-8390-2a97a44f7d09" />
</div>

5. Amazon Linux에는 기본적으로 설치 : 다른 OS에서는 별도로 설치 필요
6. Demo
   - EC2 인스턴스 2개 생성
```yml
AWSTemplateFormatVersion: "2010-09-09"
Description: CloudFormation template to create 2 EC2 instances

Resources:
  MyInstance1:
    Type: AWS::EC2::Instance
    CreationPolicy: # 생성 정책
      ResourceSignal: # 신호 전송
        Timeout: PT15M
        Count: 1 # 1개 이상
    Properties:
      InstanceType: t2.micro
      ImageId: !Sub "{{resolve:ssm:/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64}}" # Amazon Linux 가져오기
      Tags:
        - Key: "Name"
          Value: !Sub "Waitcondition-instance1"
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash
          dnf install httpd -y
          service httpd start
          chkconfig httpd on
          TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
          INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
          echo "$INSTANCE_ID" >> /var/www/html/index.html
          /opt/aws/bin/cfn-signal -e $? --stack ${AWS::StackName} --resource MyInstance1 --region ${AWS::Region} # 신호를 보낸 대상은 자기 자신
      SecurityGroups:
        - Ref: WebServerSecurityGroup

  MyInstance2:
    Type: AWS::EC2::Instance
    CreationPolicy:
      ResourceSignal:
        Timeout: PT15M
        Count: 1
    Properties:
      InstanceType: t2.micro
      Tags:
        - Key: "Name"
          Value: !Sub "Waitcondition-instance2"
      ImageId: !Sub "{{resolve:ssm:/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64}}"
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash
          dnf install httpd -y
          service httpd start
          chkconfig httpd on
          TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
          INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
          echo "$INSTANCE_ID" >> /var/www/html/index.html
          /opt/aws/bin/cfn-signal -e $? --stack ${AWS::StackName} --resource MyInstance2 --region ${AWS::Region}
      SecurityGroups:
        - Ref: WebServerSecurityGroup


  WebServerSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable HTTP and SSH access
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

Outputs:
  Instance1Id:
    Description: The Instance ID of MyInstance1
    Value: !Ref MyInstance1
  Instance2Id:
    Description: The Instance ID of MyInstance2
    Value: !Ref MyInstance2
```
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : cloudformation_wait_condition.yml - demo-wait-condition
     + 보안 그룹 / EC2 인스턴스 2개 생성 (WAIT_CONDITION : 성공할 때 까지 대기 후 진행)
     + EC2 삭제

   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : instance.yml - demo-my-ec2
```yaml
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
    CreationPolicy: # 생성 정책
      ResourceSignal: # 신호 전략
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
    Metadata: # Metadata
      AWS::CloudFormation::Init:
        config:
          files: # 1. 파일 생성
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
          commands: # 2. 커맨드 실행
            00-install-agent:
              command: "./install_httpd.sh"
              cwd: "/home/ec2-user/"
            00-cfn-signal: # 3. cfn-signal 전송
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

   - 파일 생성 확인 : 요청한 내용 반영
```
sudo -s
dir
nano install_httpd.sh
```

   - CloudFormation 삭제 (리소스 삭제)
