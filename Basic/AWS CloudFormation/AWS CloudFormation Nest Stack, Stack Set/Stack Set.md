-----
### Stack Set
-----
1. 하나의 템플릿으로 다양한 리전 및 계정에 동시에 프로비전 및 관리 가능 (업데이트 / 삭제 가능)
2. 별도의 역할을 활용하여 프로비전 (IAM 사용자 역할과 별도) : 프로비전에 사용할 역할과 해당 계정에서 사용할 역할의 분리
3. 다양한 설정 지원
   - 리전 선택 / 계정 선택
   - 동시 프로비전 스택 숫자
   - 배포 방식 (순차 / 병렬)
<div align="center">
<img src="https://github.com/user-attachments/assets/cb52600c-2bff-4880-81d0-0cb8414ab375" />
</div>

4. Demo
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : stackset_roles.yml - demo-stackset-role (IAM 역할 프로비전)
```yml
AWSTemplateFormatVersion: "2010-09-09"
Description: CloudFormation Template for Creating IAM Roles for CloudFormation

Resources:
  # 1. CloudFormationAdminRole (어떤 역할에 접근할 수 있는지 설정)
  CloudFormationAdminRole:
    Type: "AWS::IAM::Role"
    Properties:
      RoleName: "CloudFormationAdminRole"
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: "Allow"
            Principal:
              Service: "cloudformation.amazonaws.com"
            Action: "sts:AssumeRole"
      Policies:
        - PolicyName: "AdminAccessPolicy"
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              - Effect: "Allow"
                Action: "*"
                Resource: "*"

  # 2. AWSCloudFormationStackSetExecutionRole (CloudFormation이 실제로 리소스 프로비전 시 사용하는 Role)
  AWSCloudFormationStackSetExecutionRole:
    Type: "AWS::IAM::Role"
    Properties:
      RoleName: "AWSCloudFormationStackSetExecutionRole"
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: "Allow"
            Principal:
              AWS: !Sub "arn:aws:iam::${AWS::AccountId}:root"
            Action: "sts:AssumeRole"
      ManagedPolicyArns:
        - "arn:aws:iam::aws:policy/AdministratorAccess"

Outputs:
  CloudFormationAdminRoleOutput:
    Description: "IAM Role CloudFormationAdminRole"
    Value: !Ref CloudFormationAdminRole

  AWSCloudFormationStackSetExecutionRoleOutput:
    Description: "IAM Role AWSCloudFormationStackSetExecutionRole"
    Value: !Ref AWSCloudFormationStackSetExecutionRole
```

   - CloudFormation - StackSet - StackSet 만들기
     + IAM 관리자 역할 (IAM 역할 이름 : CloudFormationAdminRole) / IAM 실행 역할 이름 : AWSCloudFormationStackSetExecutionRole
     + 준비된 템플릿 : 템플릿 파일 업로드 (instance_stackset.yml)
     + 이름 : demo-my-stackset
     + 계정에 스택 배포 : 계정ID 입력
     + 리전 지정 : US-EAST-1, US-WEST-1, AP-Northeast-1
     + 최대 동시 계정 : 3 / 내결함성 : 3 (적을수록 안전)
     + 리전 동시성 : 병렬적 (동시 배포)
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

   - 총 3개의 선택 동시에 프로비전 : 각각의 스택으로 발생
   - 각 리전마다 EC2 인스턴스 생성된 것 확인
   - 작업 - Stack 업데이트 및 삭제
     + 계정 번호 입력 필요
     + 리전 지정 필요
     + 최대 동시 게정 지정 필요 (1 설정)
     + 작업 - Pending 상태 확인
     
