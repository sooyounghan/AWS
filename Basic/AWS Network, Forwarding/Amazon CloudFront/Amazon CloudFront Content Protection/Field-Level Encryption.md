-----
### Field-Level Encryption
-----
1. CloudFront를 활용해 실제 데이터를 처리하는 주체끼리 데이터를 암호화해서 전달할 수 있는 방법 : HTTPS 통신 과는 별도 개념
2. Edge Location에서 받은 데이터 중 특정 데이터를 주어진 Public Key로 암호화
3. 이후 데이터를 처리하는 측에서 Private Key로 복호화하여 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/9a470b34-da8b-4f1e-b8f2-628c71bf16a6" />
</div>
