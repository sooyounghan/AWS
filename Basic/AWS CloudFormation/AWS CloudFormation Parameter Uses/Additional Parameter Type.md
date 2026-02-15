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
