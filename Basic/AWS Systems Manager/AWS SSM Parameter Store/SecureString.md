-----
### SecureString
-----
1. KMS를 활용해서 파라미터를 암호화하여 저장
   - KMS Managed Key를 선택해 암호화 (기본 AWS / SSM 키)
   - 조회 시 WithDecryption 옵션을 넣어주어야 복호화 된 값을 받을 수 있음

2. 조회 권한과 Decryption 권한이 분리되어 있음 : 조회하려면 KMS 권한도 필요
