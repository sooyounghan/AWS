-----
### 파라미터 순서 및 규칙 정의
-----
1. CloudFormation의 파라미터를 입력받을 때, 그룹 / 라벨링 / 순서 조정 가능
2. Metadata Section 활용
3. Demo
```yml
AWSTemplateFormatVersion: "2010-09-09"
Description: CloudFormation Template to Demonstrate Parameter Types and Ordering

Metadata: # Metadata 섹션
  AWS::CloudFormation::Interface:
    ParameterGroups:
      - Label:
          default: "General Configuration"
        Parameters:
          - ProjectName
          - Environment
          - InstanceType
      - Label:
          default: "Network Configuration"
        Parameters:
          - VpcId
          - SubnetIds
          - SecurityGroupIds
      - Label:
          default: "AMI and Key Pair"
        Parameters:
          - AmiId
          - KeyPairName
      - Label:
          default: "Advanced Settings"
        Parameters:
          - InstanceCount
          - CustomTag
    ParameterLabels:
      ProjectName:
        default: "Project Name"
      Environment:
        default: "Deployment Environment"
      InstanceType:
        default: "EC2 Instance Type"
      VpcId:
        default: "VPC ID"
      SubnetIds:
        default: "Subnet IDs"
      SecurityGroupIds:
        default: "Security Group IDs"
      AmiId:
        default: "AMI ID"
      KeyPairName:
        default: "EC2 Key Pair"
      InstanceCount:
        default: "Number of EC2 Instances"
      CustomTag:
        default: "Custom Tag for EC2 Instances"

Parameters:
  ProjectName:
    Type: "String"
    Default: "MyProject"
    Description: "The name of the project."
    MinLength: 3
    MaxLength: 50

  Environment:
    Type: "String"
    AllowedValues: ["Development", "Staging", "Production"]
    Default: "Development"
    Description: "The environment to deploy to."

  InstanceType:
    Type: "String"
    Default: "t3.micro"
    AllowedValues: ["t3.micro", "t3.small", "t3.medium", "m5.large"]
    Description: "The EC2 instance type."

  VpcId:
    Type: "AWS::EC2::VPC::Id"
    Description: "The VPC ID to launch resources in."

  SubnetIds:
    Type: "List<AWS::EC2::Subnet::Id>"
    Description: "The list of subnet IDs for EC2 instances."

  SecurityGroupIds:
    Type: "List<AWS::EC2::SecurityGroup::Id>"
    Description: "The security group IDs for the EC2 instances."

  AmiId:
    Type: "AWS::EC2::Image::Id"
    Description: "The AMI ID for the EC2 instances."

  KeyPairName:
    Type: "AWS::EC2::KeyPair::KeyName"
    Description: "The key pair to SSH into the EC2 instances."

  InstanceCount:
    Type: "Number"
    Default: 2
    MinValue: 1
    MaxValue: 10
    Description: "The number of EC2 instances to launch."

  CustomTag:
    Type: "CommaDelimitedList"
    Default: "Name=MyInstance"
    Description: "A custom tag to assign to the EC2 instances (e.g., Name=MyInstance,Env=Dev)."

Resources:
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
   - CloudFormation - 템플릿 파일 업로드 : parameter_order.yml - 파라미터 확인
     + General Configuration
     + Network Configuration
     + AMI and Key Pair
     + Advanced Settings
     + 각 항목 마다 세부 내용 지정 
