-----
### Session 
-----
1. OSI 7 Model(과거)과 TCP / IP Model(현대) 구조와 과거 Mainframe 시절 통신
<div align="center">
<img src="https://github.com/user-attachments/assets/bb0ca5ef-f09a-4b12-b4c4-e047d584ec53" />
<img src="https://github.com/user-attachments/assets/d7fa4637-ef3d-497e-8b5e-02d6bed7341f" />
</div>

   - 과거 : Terminal 1에서 Terminal B의 통신 구분 불가 (Mainframe 상 통신이므로, IP와 Port 주소가 전부 동일)
   - 즉, Layer 1, 2, 3, 4 상에는 통신 자체를 구분할 수 없음

2. Session Layer
   - 통신 주체끼리 연결을 유지할 수 있는 방법 정의
   - 예전 컴퓨팅 환경에서 Layer 1, 2, 3, 4 이외의 차원에서 지속적 연결(세션)이 수립될 수 있는 방법 제공 (예) MAC Address(2)와 IP(3) 주소와 포트(4)가 동일한 상황에서 유저를 구분하는 방법은 무엇인가?)
   - 현재도 마찬가지로 Layer 4 이상의 추가적 차원에서 지속적 연결(세션)을 수립할 수 있는 방법을 포함 (예) HTTP Cookie)
<div align="center">
<img src="https://github.com/user-attachments/assets/48d78ad0-fdaf-4610-8ca2-261360e219a8" />
</div>

   - 일부 프로토콜의 경우 Session Layer 자체를 구현하지 않음 (예) FTP)

3. HTTP와 FTP
   - HTTP
<div align="center">
<img src="https://github.com/user-attachments/assets/b9e69f56-e57b-45a3-b43e-e37097ead045" />
</div>


   - FTP
<div align="center">
<img src="https://github.com/user-attachments/assets/718541b5-ff27-4e70-8f9a-9746d3e62517" />
</div>

4. Presentation Layer
   - 받은 데이터를 해석하는 방법 정의 : 파싱, 압축 해제, 복호화 등 Application Layer에서 사용할 수 있는 방식으로 변환 담당
<div align="center">
<img src="https://github.com/user-attachments/assets/a0a57a55-ced7-483e-b81e-8a9c5e9c63f5" />
<img src="https://github.com/user-attachments/assets/d588d7f2-8609-4a33-a740-744f696a6c7c" />
<img src="https://github.com/user-attachments/assets/3b559941-e510-4c25-8f68-5038244e3d6f" />
<img src="https://github.com/user-attachments/assets/36c70763-bc5f-443e-bebb-709acff45a26" />
<img src="https://github.com/user-attachments/assets/f9e64fea-5c55-4831-9f1c-61ae398c0779" />
<img src="https://github.com/user-attachments/assets/bbc9d89a-4e56-4255-ae96-55d5d1665503" />
</div>

5. Application Layer
   - 실제 받은 데이터를 처리하는 방법 정의 : 데이터를 가지고 무엇을 어떻게 처리할지에 관한 레이어
   - 예) HTTP
     + Method (GET / POST / PUT / DELETE / HEAD / OPTION / PATCH)
     + Status Code (2xx, 3xx, 4xx, 5xx)
     + Header
       * Host
       * User-Agent
       * Authorizations
       * Accept-Encoding
       * Content-Type

6. Application Load Balancer
   - L7 Load Balancer
   - 호스트 기반 라우팅 (즉, 도메인을 확인하여 이를 기반으로 의사 결정)
<div align="center">
<img src="https://github.com/user-attachments/assets/3cffcf7a-23f7-4467-9d1d-f0f8a4714057" />
<img src="https://github.com/user-attachments/assets/6a3e141f-3f8e-43d7-a0e2-eb5ee7439f36" />
</div>
