-----
### Output 섹션
-----
1. 리소스가 프로비전 된 후 필요한 정보를 스택 바깥으로 내보내는 섹션 : 내보낸 정보는 다른 스택에서 참조해서 쓰거나 콘솔에서 확인 가능
2. 구성
   - Logical ID
   - 설명
   - 값
   - 이름
<div align="center">
<img src="https://github.com/user-attachments/assets/bc8401a9-9251-4c81-82eb-64652e8464e2" />
</div>

```yml
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0182f373e66f89c85
    us-west-1:
      AMI: ami-025258b26b492aec6
    ap-northeast-2:
      AMI: ami-0023481579962abd4

Parameters:
  # LatestLinuxAmiId:
  #   Type: "String"
  #   Default: ami-0023481579962abd4
  InstanceName:
    Type: "String"
    Default: "MyInstance"
  InstanceType:
    Description: "EC2 instance type."
    Type: "String"
    Default: "t3.micro"
    AllowedValues: ["t3.micro", "t3.small", "t3.medium", "t2.micro"]
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: !Ref InstanceName
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", AMI]
      InstanceType: !Ref InstanceType
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
