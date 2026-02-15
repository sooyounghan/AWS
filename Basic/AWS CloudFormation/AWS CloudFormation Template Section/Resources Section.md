-----
### Resources 섹션
-----
1. 스택에서 실제로 프로비전 할 리소스를 정의하는 유일한 필수 섹션
2. 리소스의 구성
   - Logical Nanem : 스택 안에서 리소스를 구분하기 위한 아이디
     + 실제 리소스 이름 / 아이디와 다른 개념
     + 스택 안에서만 통용 (일종의 반 번호)

   - 유형 : 리소스의 유형 (예) S3, EC2, Elastic IP 등)
     + AWS::ProductIdentifier::ResourceType 형식 (예) AWS::EC2::Instance```

   - 리소스 속성 (Resource Attribute) : 리소스 자체 공통적 속성 정의 (삭제, 업데이트 리소스 전후 관계 등 정의)
   - 속성 (Property) : 리소스 유형별로 자세한 속성 정의
     + 예) EC2 속성 : EC2 인스턴스 유형, AMI, 보안그룹, EBS 설정, 태그(이름) 등
     + 다른 리소스 혹은 받은 파라미터를 참조하여 설정 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/6ae2ff0b-e196-43dd-a7ef-fc89fdae5eff" />
<img src="https://github.com/user-attachments/assets/3e0753f9-094b-4ced-bc59-b0bf759b7ba3" />
</div>
