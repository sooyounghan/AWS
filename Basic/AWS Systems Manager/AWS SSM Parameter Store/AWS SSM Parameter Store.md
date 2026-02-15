-----
### AWS SSM Parameter Store
-----
1. AWS에서 주요 설정과 값들을 저장 / 관리 / 활용하기 위한 서비스 (예) API 주소, DB 호스트명, AMI ID, API Token, 유저아이디 / 패스워드, 환경변수 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/9a04f912-8108-4e0c-8048-36ec1cb18025" />
</div>

2. Key-Value 기반으로 필요한 값을 저장하고 불러오기 가능
   - 예) /myproject/prod/db/userid : "my_user_id"
   - 저장 가능한 값의 형식
     + String : 텍스트
     + StringList : 컴마(,)로 구분된 값 (예) Monday, Wednesday, Friday)
     + SecureString : KMS 기반으로 암호화된 텍스트

3. IAM으로 권한 관리 가능
   - 접근 권한 (계층 구조 적용 가능)
   - 복호화 권한 (SecureString)

4. 다양한 AWS 서비스에서 활용 (예) Lambda, EC2, CloudFormation 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/34400849-c444-4329-b802-3adb33ef5055" />
<img src="https://github.com/user-attachments/assets/515f87b0-b626-463f-9220-46cbbf1edcf7" />
<img src="https://github.com/user-attachments/assets/f05ea2ac-64dc-40dc-9f0e-c07a8e5696b1" />
<img src="https://github.com/user-attachments/assets/f9f95b65-121b-429b-9240-eb2607e728f8" />
<img src="https://github.com/user-attachments/assets/748db133-5236-4c21-8cee-66eece6a070e" />
<img src="https://github.com/user-attachments/assets/b9a2bfed-c5fa-4323-8706-6cd12245d103" />
</div>

5. Tier : 두 가지 Tier
<div align="center">
<img src="https://github.com/user-attachments/assets/3822b4b5-214a-4ac1-87ce-b274c678d275" />
</div>
