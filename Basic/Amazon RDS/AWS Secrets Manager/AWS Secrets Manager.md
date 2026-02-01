-----
### AWS Secrets Manager
-----
1. 애플리케이션, 서비스, IT 리소스에 액세스를 할 때 필요한 보안 정보를 보호하는데 도움
2. 이 서비스를 이용하면 수명 주기에 걸쳐 데이터베이스 자격 증명, API 키 및 다른 보안 정보를 손쉽게 교체 및 관리, 검색 가능
3. 보안 정보(암호, API Key 등)를 안전하게 저장하고 손쉽게 사용할 수 있도록 도와주는 서비스 : RDS, RedShift 등 암호를 안전하게 저장하고 관리
4. 보안 정보의 주기적인 교체(Rotation) 지원 : 일정 주기로 자동으로 RDS 암호를 교체 및 데이터베이스에 지원
5. CloudFormation 등 다른 서비스와 연동하여 안전한 보안 확보 가능
6. RDS 및 RDS 관련 서비스 (RDS Proxy 등)과 연동
<div align="center">
<img src="https://github.com/user-attachments/assets/aa2eb207-29f1-4632-a505-4d0dc3a7142f" />
<img src="https://github.com/user-attachments/assets/c77c7555-9aeb-4046-a1d5-739f11e3b180" />
<img src="https://github.com/user-attachments/assets/f62aaa36-ccf1-4a62-9e62-5605f903a8bc" />
<img src="https://github.com/user-attachments/assets/52250f9b-4b1f-4913-9581-1b453c9d4ffe" />
<img src="https://github.com/user-attachments/assets/53f68a3b-b63d-4a8c-b2b8-d6f22743e5f9" />
<img src="https://github.com/user-attachments/assets/ea0f34a3-9d09-4838-bad1-4689286e3150" />
</div>

7. 보안 정보를 저장하고 불러오는 데 비용 발생 : Parameter Store와 가장 큰 차이점으로는 Parameter 경우 무료 (10,000개 이하)
8. 바로 삭제 불가능 : 최소 7일 대기 필요
