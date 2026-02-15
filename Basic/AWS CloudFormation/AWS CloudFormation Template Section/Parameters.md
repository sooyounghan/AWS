-----
### Parameters
-----
1. 템플릿을 커스터마이징 하기 위해 설정 가능한 값
2. 두 가지 종류
   - 일반 파라미터 : 템플릿 프로비전 시 사용자에게 받는 값으로 Parameter Section에서 정의
     + 예) EC2 Instance 타입, AMI ID / CloudFront DNS 값 / RDS DB 기본 패스워드 등
     + 활용 예시 : 하나의 정형화 된 아키텍쳐를 찍어낼 때 DNS만 교체 혹은 인스턴스 타입만 교체

   - Pesudo 파라미터 : CloudFormation에서 템플릿 프로비전 시 지정해주는 값으로 프로비전 당시의 상황을 반영하여 스택으로 자동으로 전달
     + 예) Region, Account ID, StackName
     + 활용 예시 : S3 버킷 이름 끝에 Account ID를 붙이고 싶은 경우

3. Parameters 섹션
   - 템플릿에서 일반 파라미터를 정의하는 섹션
   - 형식
     + Logical ID : 스택 내에서 통용되는 파라미터 이름
     + 타입 : String, Number, ```List<Number>``` 등 + AWS 지정 타입으로 AWS 리소스 지정 가능 (예) Subnet, EC2 Keypair, AMI ID 등)
     + 설명 : 파라미터 설명
     + Default : 기본값
     + Allowed Value : 허용 가능한 값
     + 기타 : 파라미터 조건(길이, 정규식 등), NoEcho(암호 등 활용)

   - ```!Ref Intrinsic Function```으로 참조

4. Intrinsic Function
   - CloudFormation에서 지원하는 기본 함수로, 스택 관리를 위한 다양한 기능 제공 : Resource 속성 / Outputs / Metada / Update Policy 에서만 사용 가능
   - 두 가지 호출 방식
     + Full Function : Fn:: 함수명
     + Short Form : !함수명
     + 예) Ref : 파라미터 혹은 리소스를 참조하기 위한 함수 ("Ref : Logical ID" / !Ref Logical ID)

   - 주요 사용 사례 : 다른 리소스 참조, AWS 계정의 AZ 목록 확보, 리소스 속성 불러오기 등
<div align="center">
<img src="https://github.com/user-attachments/assets/8e50e7fd-d75c-4750-ab12-d1b0cc132130" />
</div>

5. CloudFomration Pesudo 파라미터
   - CloudFormation에서 템플릿 프로비전 시 동적으로 넣어주는 파라미터
   - 종류
     + AWS:AccountID - 리소스가 프로비전되는 계정의 Account ID
     + AWS::Region - 리소스가 프로비전되는 계정의 리전
     + AWS::StackName - 리소스가 프로비전되는 계정의 스택 이름

   - 주요 사용 사례
     + 리소스의 이름 등에 계정명 혹은 리전명등을 포함하여 중복된 이름이 나오지 않도록 설정
     + 프로비전 시점에 다양한 리소스 조회

   - !Ref 혹은 !Sub로 활용 (!Sub : 특정 값들(파라미터, 다른 함수 등)로 스트링을 만드는 함수)
<div align="center">
<img src="https://github.com/user-attachments/assets/647e02a8-bd3a-4a8d-bb3d-84d7a8ebfaa1" />
</div>
