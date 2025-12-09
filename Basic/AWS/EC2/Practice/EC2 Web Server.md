-----
### 실습 : EC2로 웹 서버 만들기
-----
1. Amazon EC2 Provision : t2.micro 타입(Free-Tier)
   - Region : 서울 확인
   - 인스턴스 시작 : Demo-My-Webserver

2. AMI 선택 (Amazon Linux 3) 및 키 페어(로그인)은 키 페어 없이 계속 진행 선택
  
3. 보안 그룹 설정 (80, 22 포트 허용)
   - 보안 그룹 생성 : 인터넷에서 HTTP 트래픽 허용 체크
    
4. Apache 설치 및 설정
   - EC2 인스턴스로 웹 콘솔을 사용해 연결 : EC2 인스턴스 연결
   - 관리자 접근 : sudo -s
   - Apache 설치 : dnf install httpd -y
   - Apache Web Server 실행 : start httpd.service

5. 웹 서버 확인
   - 자동으로 웹 서버가 실행되도록 설정 : chkconfig httpd on
   - Public IP로 접속했을 때, 웹 페이지 메세지 변경 : nano /var/www/html/index.html 명령어 실행 후 변경 (Ctrl + X + Y) 후, 새로고침하면 변경
   - 퍼블릭 IPv4 DNS로 접속해도 동일한 결과 확인 가능

6. AMI 생성
   - 인스턴스 선택 후 우클릭 - 이미지 및 템플릿 - 이미지 생성
   - demo-my-webserver-ami
   - 새로운 태그 추가
     + 키 : Name
     + 값 : demo-my-webserver-ami

7. 기존 인스턴스 종료

8. AMI로부터 새로운 EC2 인스턴스 생성
   - 우측 메뉴 이미지 - AMI 클릭 - 해당 AMI 우클릭 - AMI로 인스턴스 시작
   - 이름 : demo-my-webserver-clone
   - AMI는 방금 전 생성했던 직접 만든 AMI 선택
   - 키 페어(로그인)은 키 페어 없이 계속 진행 선택
   - 보안 그룹 생성 : 인터넷에서 HTTP 트래픽 허용 체크
     
9. 신규 인스턴스 확인 및 종료
