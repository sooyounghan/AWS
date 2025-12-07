-----
### AWS 사용과 인증
-----
1. AWS 이용 방법
<div align="center">
<img src="https://github.com/user-attachments/assets/610f1b8a-6f6d-4074-8e50-910757177b19" />
</div>

   - AWS 웹 콘솔
     + 웹 기반의 콘솔에 로그인하여 AWS를 사용
     + 콘솔 액세스 자격 증명을 사용하여 인증

   - 프로그래밍 액세스 방식
     + Command Line Interface(CLI) : 명령줄 기반으로 AWS 서비스를 관리하는 도구
     + Software Devlopment Kit(SDK) : 다양한 프로그래밍 언어로 만들어진 AWS 서비스를 관리하는 패키지 (프로그래밍 언어에 맞는 SDK를 사용해 개발에 활용)
     + 프로그램 방식 액세스 자격 증명을 사용하여 인증
<div align="center">
<img src="https://github.com/user-attachments/assets/a32537cb-43b2-420c-88dd-ac361937a238" />
</div>

2. 콘솔 액세스 자격 증명 : 콘솔 로그인을 위해 필요
   - Root Email / Password : Root 사용자 로그인을 위해 사용
   - IAM 사용자 이름 / Password : IAM 사용자가 로그인 하기 위해 사용
   - Multi-Factor Authentication (MFA) : 다른 자격 증명에 보안을 강화하기 위한 임시 비밀번호

3. 프로그램 방식 액세스 자격 증명 : AWS나 CLI 혹은 SDK를 사용할 때 필요한 자격 증명
   - Access Key 구성
     + Access Key ID : 유저 이름에 해당하는 키 (일반적으로 공개되어도 무방)
     + Secret Access Key : 패스워드에 해당하는 키 (공개되면 안 됨)
   - 하나의 IAM 사용자 당 2개의 Access Key Pair 발급 가능
   - 활성화 / 비활성화 가능
   - Secret Access Key의 경우, 발급 시점 이외에 다시 확인이 불가능
   - 생성한 IAM 사용자의 권한을 행사 가능
   - IAM 사용자를 서버 / 프로그램 별로 생성하여 Acceess Key를 전달하여 CLI / SDK 사용
     + AWS CLI의 경우 서버의 별도 파일에 자격 증명을 저장하여 인증
     + AWS SDK의 경우 별도 파일에 자격 증명을 저장하거나 런타임에 자격 증명을 명시해 인증
   - Profile : 자격 증명 단위로, 서로 다른 자격 증명에 이름을 부여해서 필요할 때마다 스위칭 가능

4. 정리
   - AWS 이용 방법
     + AWS 웹 콘솔 : 콘솔 액세스 자격 증명을 사용해 인증
     + 프로그래밍 액세스
       * Command Line Interface (CLI)
       * Software Development Kit (SDK)
       * 프로그램 방식 액세스 자격 증명을 사용하여 인증

   - 콘솔 액세스 자격 증명
     + Root Email / Password
     + IAM 사용자 이름 / Password
     + Multi-Factor Authentication (MFA)
    
   - 프로그램 방식 액세스 자격 증명
     + Access Key Pair
     + 하나의 IAM 사용자 당 2개의 Access Key 발급 가능
