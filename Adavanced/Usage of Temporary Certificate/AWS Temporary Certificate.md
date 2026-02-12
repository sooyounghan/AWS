-----
### AWS Temporary Certificate
-----
1. AWS 서비스(STS) 등에서 발급하는 유효기간이 있는 자격증명 (15분에서 12시간까지 유효기간 후 만료)
2. 특징
   - 동적으로 생성되서 사용 : 하드코딩이 아닌 필요한 시점에서 생성해서 활용, 이후 필요에 따라 재발급 (보안이 뛰어남)
   - 지속기간이 정해져 있으므로 로테이션 필요가 없음
   - IAM 유저 외에 다양한 Identity에게 자격 증명 부여 가능 : Facebook, Google, OpenID, 회사 조직 같이 다양한 Identity에게 권한 부여 가능
   - 관리가 쉬움 : 동적으로 생성되고 자동으로 로테이션 되므로 권한의 부여 및 회수가 쉬움
   - 하나의 IAM 사용자 혹은 주체에서 여러 자격증명 생성 가능

3. Access Key Pair와 다른 점
   - Access Token 포함
   - Aceess Key ID가 'ASIA'로 시작
<div align="center">
<img src="https://github.com/user-attachments/assets/1b603c40-ffd0-4cf6-b542-f095fc217488" />
</div>

4. AWS의 임시자격증명 생성
   - IAM User, Web Identity(예) Facebook 유저)가 특정 Role을 Assume하여 자격증명 생성
   - AWS STS(Security Token Service)의 Assume Role
     + Assume이 가능하다면, 해당 Role의 모든 권한 행사 가능
     + 단, 해당 역할의 신뢰정책에서 허용 필요
   - Identity Federation(OpenID, SMAL 등)에서 생성

5. AssumeRole
<div align="center">
<img src="https://github.com/user-attachments/assets/3b76b01e-ec29-4c5a-8ac7-1437fe9f94a7" />
<img src="https://github.com/user-attachments/assets/976f8d92-abef-4d4e-8c2b-6afaf1986873" />
<img src="https://github.com/user-attachments/assets/dd9cd2e4-fdba-4f72-86d9-3b3d8dfe06bd" />
</div>

   - 역할 (Role)
<div align="center">
<img src="https://github.com/user-attachments/assets/5108a19d-9a83-4343-8e39-41fbffc191f2" />
<img src="https://github.com/user-attachments/assets/cd3c32c7-0098-49bb-9111-c7148c0e2f87" />
</div>

   - 신뢰 관계
<div align="center">
<img src="https://github.com/user-attachments/assets/8833d5a0-573c-4b5b-985a-fbeb5419b8b2" />
<img src="https://github.com/user-attachments/assets/d3556f75-286b-411d-8047-f6f8acfa9ae0" />
</div>

   - AssumeRoleWithWebIdentity
<div align="center">
<img src="https://github.com/user-attachments/assets/89bedc91-1a4f-4dae-a87a-1e3e68e4593c" />
</div>

   - 예시) 교차 계정 Assume Role
<div align="center">
<img src="https://github.com/user-attachments/assets/f7e91c7c-e9c9-489f-8e36-fca54d6f9611" />
</div>

   - 예시) Identity Center
<div align="center">
<img src="https://github.com/user-attachments/assets/9db9da3f-32f7-4616-8047-8285639418be" />
</div>

6. AWS의 임시자격증명 사용
   - SDK / CLI : 기본적으로 Access Key Pair와 동일하나 토큰 부분만 추가
<div align="center">
<img src="https://github.com/user-attachments/assets/76a58c5e-2bb3-450b-be3f-37f6529069e7" />
<img src="https://github.com/user-attachments/assets/8d0bb2a0-9db1-4d43-ae4e-a6b07587b94c" />
</div>

   - EC2 역할 부여
<div align="center">
<img src="https://github.com/user-attachments/assets/96fcfc09-03c5-4e79-8240-a901f00ad2e9" />
<img src="https://github.com/user-attachments/assets/1356e45e-87c6-4c99-9812-3169104918cc" />
</div>

   - HTTPS API Requests : 본래대로 Request에 Sign 후, HTTP Header / QueryString에 (X-Amz-Security-Token) 토큰 추가
<div align="center">
<img src="https://github.com/user-attachments/assets/2f32ff97-8b01-4c7b-8a43-3cedd3b3195a" />
</div>

-----
### 영구자격증명 vs 임시자격증명
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/3b8b2a9e-9481-4738-b0b7-6a02e0ec5b73" />
</div>

