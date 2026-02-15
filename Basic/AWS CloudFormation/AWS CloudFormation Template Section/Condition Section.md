-----
### Condition 섹션
-----
1. 조건에 따라 리소스 생성의 의사결정 가능
   - 예) Dev 리소스라면, 더 작은 타입의 EC2 인스턴스
   - 예) Prod 라면, Multi-AZ로 DB 구성
2. !If Intrinsic Function으로 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/5ec316e1-f833-4c89-822d-7d0839b0fb1b" />
</div>

```yml
Parameters:
  LatestLinuxAmiId:
    Type: "String"
    Default: ami-0023481579962abd4
  InstanceName:
    Type: "String"
    Default: "MyInstance"
  EnvironmentType:
    Description: Type of environment (prod or dev)
    Type: String
    Default: dev
    AllowedValues:
      - prod
      - dev
    ConstraintDescription: must be either prod or dev.
Conditions:
  IsProd: !Equals [!Ref EnvironmentType, prod]
  IsDev: !Equals [!Ref EnvironmentType, dev]

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: !Ref InstanceName
      ImageId: !Ref LatestLinuxAmiId
      InstanceType: !If
        - IsProd
        - t3.large # Use larger instance in production
        - t2.micro # Use smaller instance in development
      SecurityGroups:
        - !Ref SSHSecurityGroup
      AvailabilityZone:
        Fn::Select: ["0", Fn::GetAZs: !Ref "AWS::Region"] # or !Select [0, !GetAZs ""]
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub "mybucket-${AWS::AccountId}-${AWS::StackName}"
  MyEIP:
    Type: AWS::EC2::EIP
    Properties:
      InstanceId: !Ref MyInstance
      Tags:
        - Key: "Name"
          Value: !Ref InstanceName
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
