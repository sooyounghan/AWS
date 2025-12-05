-----
### 암호화 및 SSL / TLS
-----
1. 암호화 : 수학적 과정으로 어떤 정보를 읽을 수 없는 데이터로 변환하는 행위 (복원이 가능한 것과 불가능 한 것 존재(Hash))
2. 암호화의 종류
   - 암호화 기술을 사용하는 상황
     + 저장된 데이터 보호 (Encryption At Rest)
     + 전송 중 데이터 보호 (Encryption At Transit)

   - 암호화 방법
     + 대칭키 암호화
     + 비대칭키 암호화

3. 암호화 용어
   - 평문 (Plaintext) : 암호화 하기 전 데이터
   - 암호문 (Ciphertext) : 평문을 암호 키와 알고리즘을 사용해 암호화 한 데이터
   - 암호화 (Encryption) : 평문에 암호화 알고리즘과 키를 적용하여 키를 소유한 주체가 아니면 알아볼 수 없는 암호문으로 만드는 과정
   - 복호화 (Decryption) : 암호문에 키와 복호화 알고리즘을 적용해 평문으로 되돌리는 과정
   - 키 (Key) : 평문을 암호화하거나 암호문을 평문으로 돌리기 위한 알고리즘에 핵심 가변정보 값
   - 암호 알고리즘 : 암호화 / 복호화를 위해 사용되는 알고리즘 (AES, DES 등)

4. 저장된 데이터 보호 (Encryption At Rest)
<div align="center">
<img src="https://github.com/user-attachments/assets/a0580206-ac51-4537-b462-003867d455c5" />
</div>

  - 데이터를 저장할 때 암호화하고, 필요할 때 복호화해서 사용하는 방식
  - 주로 하나의 물리적 기기에 보안을 적용하기 위해 사용 (예) 기기를 탈취당했을 때, 데이터 보호)
  - 주로 키 파일 혹은 암호를 사용하여 암호화 및 복호화

5. 전송 중 데이터 보호 (Encryption At Transit)
<div align="center">
<img src="https://github.com/user-attachments/assets/62606209-8356-4ffe-bae1-b5447bfcf21b" />
</div>

   - 데이터 전송 중 암호화를 적용하여 데이터가 탈취 당하지 않도록 보호
   - 주로 여러 시스템 / 기기 간에 보안을 적용하기 위해 사용 (예) SSH / TLS, HTTPS 등)

6. 대칭키 암호화 (Symmetric Encryption)
   - 하나의 키로 암호화, 복호화 하는 알고리즘
   - 연산이 빠르며, 키를 전달하는 방법에 대한 고민이 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/d1efb529-db4c-45b7-924a-c63c545a56a6" />
<img src="https://github.com/user-attachments/assets/53685667-9afa-4a45-9fd1-1b6bb119c60f" />
<img src="https://github.com/user-attachments/assets/dcc8dcf9-76c4-4e0f-a673-b629e8662572" />
</div>

7. 비대칭키 암호화 (Asymmetric Encryption)
   - 한 쌍의 키를 활용한 암호화, 단 하나의 암호는 암호화만 가능하며, 다른 하나는 복호화만 가능
   - 연산이 비교적 복잡하지만 키 전달이 쉬움
   - 공개 키 / 비밀 키 : 각각 암호화 / 복호화만 할 수 있는 키 쌍
     + 예) 공개키로 암호화하면 비밀 키로 복호화 가능
     + 예) 비밀키로 암호화하면 공개 키로 복호화 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/dd7b5941-991b-45c4-bddc-8a1bb2bbdef4" />
<img src="https://github.com/user-attachments/assets/d418c3a3-bba6-4f96-a067-4c2197f998ad" />
</div>

8. 암호화 서명 (Signing)
   - 키를 사용해서 데이터 생성자가 데이터를 생성했음을 보장하는 방법
     + Private 키를 사용해 Public 키로 검증 가능한 데이터 서명(Signature) 생성
     + Private 키를 가진 주체가 데이터를 생성했음을 보장하는 방법
<div align="center">
<img src="https://github.com/user-attachments/assets/d552b8e5-57af-4412-921c-f02bccc894c8" />
<img src="https://github.com/user-attachments/assets/4fc597fa-3806-4935-b660-70a1dd1ee069" />
<img src="https://github.com/user-attachments/assets/38b097af-a78f-4d91-b93a-512ecc4136c5" />
</div>

9. SSL / TLS
   - 클라이언트와 서버 간 데이터 무결성과 기밀성을 보장할 수 있는 프로토콜 : 상호 간 통신은 암호화되어 전달되며, 중간에서 데이터를 탈취해도  안전
   - HTTPS 등 다양한 프로토콜에 활용 : 즉, 일반 HTTP의 경우, 연결이 안전하지 않으므로 보안적 취약
<div align="center">
<img src="https://github.com/user-attachments/assets/893541fd-84d2-4732-9d3f-6297b7a10bb2" />
</div>

   - 3단계 과정
     + Cipher Suites 교환 (Cipher Suites : TLS에서 활용하는 보안 알고리즘들)
     + 인증 (인증서 : 믿을 수 있는 인증 기관 (Certificate Authority)에서 배포하는 서버 신원에 대한 검증 확인)
     + 키 교환
     + 통신
<div align="center">
<img src="https://github.com/user-attachments/assets/d8f674fc-21ce-4d05-b59d-a1fa05675eae" />
<img src="https://github.com/user-attachments/assets/2d0a7451-be42-4c77-8716-5f0329b195e0" />
<img src="https://github.com/user-attachments/assets/bdf7236a-668e-4451-9c33-7b9e0c78bd3e" />
<img src="https://github.com/user-attachments/assets/e68d937e-a7d3-45be-8c1d-74ef99ae6fea" />
</div>

