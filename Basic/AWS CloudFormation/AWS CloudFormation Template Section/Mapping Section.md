-----
### Mapping 섹션
-----
1. CloudFormation에서 미리 Map으로 데이터를 정의할 수 있는 섹션 : 프로비전하는 상황에 따라 알맞은 값을 선택할 수 있도록 미리 데이터 저장
2. Fn::FindInMap Instrisic Function으로 활용 : Fn::FindInMap: [ MapName, TopLevelKey, SecondLevelKey ] / !FindInMap [ MapName, TopLevelKey, SecondLevelKey ]
3. 주요 사용 사례
   - 리전별 AMI 선택
   - Route 53 Hosted Zone 선택 등
<div align="center">
<img src="https://github.com/user-attachments/assets/8c79b516-25f4-43ee-8c9c-c44a020180b8" />
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
```
