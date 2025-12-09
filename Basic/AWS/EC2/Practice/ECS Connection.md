-----
### EC2 접속 방법
-----
1. EC2 접속 방법
<div align="center">
<img src="https://github.com/user-attachments/assets/49c4ba93-fcca-4c48-9b5d-02ba28e787ae" />
</div>

  - 연결 인증 방식
  - 감사(Auditing) / 로깅 방식
  - 연결 요구사항
  - 주요 기능

2. SSH 연결
<div align="center">
<img src="https://github.com/user-attachments/assets/458821ba-e6d7-4776-bcdc-19df8fdc3ad4" />
</div>

   - SSH 키 페어
     + 인스턴스에 연결할 때(SSH / FTP) 자격 증명 입증에 사용하는 보안 자격 증명 집합 : 프라이빗 키와 퍼블릭 키로 구성
     + 다시 발급 불가능 : 분실 시, EBS를 분리해 다른 인스턴스에 연결 혹은 스냅샷을 통해 재생성 필요
     + Region 단위 : 다른 리전에서 사용하기 위해서는 Import 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/2235748d-9dc0-41d5-8b81-7091957d31bc" />
</div>

   - AMI 별 SSH 기본 유저 이름
<div align="center">
<img src="https://github.com/user-attachments/assets/d9cbc73a-e8bd-44b4-8926-19fbab69f132" />
</div>

6. FTP 연결
<div align="center">
<img src="https://github.com/user-attachments/assets/263b2290-6d77-44fc-9556-5061b10426b2" />
</div>

   - 실습 : FileZilla 설치 (MacOS / Windows 동일)
   - EC2 인스턴스 Provision : 이 때, SSH Key 생성
   - EC2 인스턴스에 HTTP 웹 서버 설치 및 구동
   - FTP로 index.html 파일 전송하여 확인
   - 인스턴스 종료

7. EC2 인스턴스 연결
<div align="center">
<img src="https://github.com/user-attachments/assets/2dd0b2c2-bffc-417e-bcff-c780664453ca" />
</div>

8. EC2 직렬 콘솔
<div align="center">
<img src="https://github.com/user-attachments/assets/d5eab1ed-66ff-47b2-9b4b-646cac26a3c8" />
</div>

   - 직렬 콘솔 활성화
   - t3.micor(무료 아님) 인스턴스 Provision
   - 접속 후 패스워드 설정
   - 직렬 콘솔 접속
   - 인스턴스 재부팅 후 확인
   - 인스턴스 종료

9. 실습
    - EC2 생성 : demo-my-ec2
    - 키 페어 생성 (키 페어 이름 : demo-my-ec2-keypair / 프라이빗 키 파일 형식 : .pem)
    - 보안 그룹 생성 (SSH 트래픽 허용, 인터넷에서 HTTP 트래픽 허용 체크)
    - sudo -s / dnf install httpd -y / service httpd start / chkconfig httpd on / nano /var/www/html/index.html
    - FileZilla 접속
      + 파일 - 사이트 관리자 - 새 사이트 - 프로토콜 : STFP / 호스트(Public IP) / 로그온 유형 : 키 파일 (사용자 : ec2-user / 키 파일은 저장한 .pem) - 연결
      + 호스트 키에 대해 캐시에 등록 체크하고, 확인 (EC2 인스턴스에 접속)
      + index.html 파일 덮어쓰기
      + EC2 Console에서 index.html 파일 확인 : dir / nano index.html
      + AWS Web에 띄우기
        * Mozilla 사용 : /var/www/html/index.html로 덮어쓰기 : 권한이 없어 Permission Denied
          * MobaXterm 사용 - Session 추가 (PublicIP,
<div align="center">
<img src="https://github.com/user-attachments/assets/c79b7743-2c40-471f-a42f-39801ef4840b" />
</div>

   - 명령어로 옮기기 : EC2에서 cp ./index.html /var/www/html

10. EC2 직렬 콘솔 실습
    - 인스턴스 생성 - 인스턴스 유형 t3.micro / 키 페어 없이 권장하지 않음 / SSH 트래픽 허용
    - 연결 - EC2 직렬 콘솔 - 계정 직렬 콘솔 허용 (이 계정은 EC2 직렬 콘솔을 사용할 권한이 없으므로 액세스 관리) - EC2 직렬 콘솔 액세스 허용 체크
    - 직렬 콘솔은 쉽게 말해 서버에 키보드를 꽂고 직접 접속하는 방법
    - User ID와 Password 필요 : EC2 인스턴스 언결
      + sudo -s
      + sudo passwd root : 암호 설정
      + 암호 입력을 하면 직렬 콘솔을 쓸 수 있음 - EC2 직렬 콘솔 - 연결
      + T3 EC2 인스턴스를 재부팅하면, 재부팅으로 인한 로그 출력 (부팅 코드, 디버그 가능) / ID, Password 입력하면 직렬 콘솔 접속 가능
        * SSH 키를 잃어버렸거나 문제로 인해 부팅이 되지 않으면 직렬 콘솔을 사용해 디버깅 가능
    
