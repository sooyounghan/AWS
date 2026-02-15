-----
### CloudFormation Help Scripts
-----
1. CloudFormation과 소통을 도와주기 위한 Python 기반 스크립트
2. cfn-signal : CloudFormation의 Wait Condition 리소스 (혹은 CreationPolicy가 붙은 리소스에 처리 완료 신호를 보내주는 스크립트)
<div align="center">
<img src="https://github.com/user-attachments/assets/19efb2a5-1c46-456c-a511-03d7d232d5eb" />
<img src="https://github.com/user-attachments/assets/5c11cf39-01f0-4454-b295-8b344ef82688" />
</div>

3. cfn-init : 리소스 메타데이터 기반으로 패키지 설치나 파일 생성 등 담당
<div align="center">
<img src="https://github.com/user-attachments/assets/9c08e824-5c1b-4a99-8e3e-ac0b375bfdeb" />
<img src="https://github.com/user-attachments/assets/56fa9877-6247-4cd7-ac26-62b1976091ce" />
</div>

4. cfn-hup : 업데이트를 체크하여 변경이 일어나면 커스텀 로직을 수행하는 스크립트
<div align="center">
<img src="https://github.com/user-attachments/assets/06444170-4402-4702-9a6b-202b5d1a1dd6" />
<img src="https://github.com/user-attachments/assets/455c5987-e819-49ca-8390-2a97a44f7d09" />
</div>

5. Amazon Linux에는 기본적으로 설치 : 다른 OS에서는 별도로 설치 필요
