-----
### S3의 객체 암호화
-----
1. 3가지 단계로 구성
   - On Transit : SSL / TLS (HTTPS)
   - At Rest (Server Side)
     + SSE-S3 : S3에서 알아서 암호화 (Default)
     + SSE-KMS : KMS 서비스를 이용해 암호화
     + SSE-C : 클라이언트에서 제공한 암호화키를 통해 암호화
     + DSSE-KMS : KMS를 활용해 Dual Layer(2계층) 암호화를 적용하는 암호화
   - Client Side : 클라이언트가 직접 암호화

2. 기본적으로 활성화 (SSE-S3)
3. 암호화 시 데이터 복제가 불가능한 경우 발생
<div align="center">
<img src="https://github.com/user-attachments/assets/aeeaf8dc-0758-4f76-950c-8d60ab4d28a6" />
<img src="https://github.com/user-attachments/assets/128096dd-8b17-447c-98e6-68b9271f7195" />
<img src="https://github.com/user-attachments/assets/be840f93-3327-4086-8d96-a2d0b7fed1d8" />
<img src="https://github.com/user-attachments/assets/ee3a6c2c-a555-4a14-9b78-efc43b288e8a" />
<img src="https://github.com/user-attachments/assets/d78d7f5d-2286-4204-9070-097cbb6dc714" />
</div>
