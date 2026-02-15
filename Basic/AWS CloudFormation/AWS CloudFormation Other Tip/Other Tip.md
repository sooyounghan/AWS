-----
### 콘솔에서는 숨겨진 리소스들
-----
1. CloudFormation에서 프로비전 시 콘솔 프로비전에서 보이지 않는 리소스 존재
2. 주로하나의 리소스와 다른 리소스를 연결해주는 숨겨진 리소스
3. 예) Instance Profile : IAM 역할과 EC2를 연결 / EBS 볼륨 맵핑
<div align="center">
<img src="https://github.com/user-attachments/assets/30d65ea8-9cb2-4884-a656-92277d417d77" />
</div>

-----
### 유용한 기능 - CloudFormation 가져오기
-----
1. 기존에 있는 리소스를 만들어진 CloudFormation 논리적 리소스(스택)으로 가져오기 가능 : 스택 간 리소스 옮기기도 가능
2. 두 가지 방법
   - IaC Generator : 기존 리소스를 기반으로 자동으로 템플릿을 생성해주는 서비스 (비추천
   - 수동 가져오기 : 수동으로 기존 리소스에 해당하는 리소스를 정의하여 가져오는 방법

3. Demo
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : instance.yml - demo-import-test
```yml
#AP-Northeast-2 리전만 사용 가능
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance"
      InstanceType: "t2.micro"
      ImageId: ami-0023481579962abd4
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
```
   - S3 - demo-my-bucket-{계정ID} : 해당 버킷을 Import
   - demo-import-test - 스택 작업 - 리소스를 스택으로 가져오기 - 템플릿 파일 업로드 : import_s3.yml
     + 리소스 식별 - 가져올 리소스 : Mybucket에 demo-my-bucket-{계정ID} 입력
     + 리소스 가져오기 (S3가 리소스에 포함되도록 Import)
```yml
#AP-Northeast-2 리전만 사용 가능
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance"
      InstanceType: "t2.micro"
      ImageId: ami-0023481579962abd4
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
  MyBucket: # Import 대상
    Type: AWS::S3::Bucket
    DeletionPolicy: Delete # 💡 삭제도 같이 삭제 (필수). 없으면 Import 불가
```
   - 리소스 정리 : 스택 삭제

-----
### 유용한 기능 - Drift Detection
-----
1. Drift : CloudFormation의 논리적 리소스와 실제 물리적 리소스가 다른 경우 : 주로, 물리적 리소스에 CloudFormation 이외의 수단 (SDK / Console / CLI)으로 변경한 경우
2. CloudFormation에서 스택의 Drift Detection 지원
   - 즉, 현재 상태기반으로 실제 리소스를 모니터링하여 변경점 확인
   - 💡 현재는 Retain으로 Delete 한 후, 다시 가져오기로만 해결 가능

3. Demo
   - CloudFormation - 스택 생성 - 템플릿 파일 업로드 : drift.yml - demo-drift
```yml
#AP-Northeast-2 리전만 사용 가능
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      Tags:
        - Key: "Name"
          Value: "MyInstance"
      InstanceType: "t2.micro"
      ImageId: ami-0023481579962abd4
      BlockDeviceMappings:
        - DeviceName: /dev/xvda
          Ebs:
            VolumeSize: 10
            VolumeType: standard
```

   - EC2 인스턴스 중지 - 설정 - 인스턴스 유형 변경 : t2.medium
   - CloudFormation - demo-drift - 스택 작업 - 드리프트 감지
     + 스택 작업 - 드리프트 결과 보기 : EC2 인스턴스 드리프트 상태 확인할 수 있음
   - CloudFormation 삭제

-----
### LLM 활용
-----
1. CloudFormation도 문서이므로 LLM을 활용해 편리하게 작성 가능
2. 리팩토링 / 주석 / 추가 리소스 등
