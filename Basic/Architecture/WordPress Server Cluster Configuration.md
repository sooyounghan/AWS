-----
### 3-Tier Architecture
-----
1. 정형화된 3개의 물리적 / 논리적 티어로 구성된 애플리케이션 아키텍쳐
2. 일반적인 티어
   - Presentation Tier : 사용자와 직접 소통하는 티어 (주로 HTML, CSS와 같은 유저를 위한 UI로 구성)
   - Application Tier : 실제 로직을 처리하는 티어 (프로그래밍 언어(PHP, Java, Python) 등으로 구성)
   - Data Tier : 데이터를 저장하는 티어 (데이터베이스 혹은 데이터를 처리하는 저장 공간으로 구성)

3. ALB / WEB EC2 / RDS 3가지 티어로 구성

-----
### Demo
-----
1. 3-Tier Architecture 아키텍쳐를 활용해 WordPress 서버를 위한 웹 서버 클러스터 구성
2. EC2 AutoScaling과 ELB로 고가용성 확보
3. EFS를 활용해 공유 스토리지에 워드프레스 설치
<div align="center">
<img src="https://github.com/user-attachments/assets/a79e2f85-9e9b-4c26-a750-2c3988ca6134" />
<img src="https://github.com/user-attachments/assets/cdd16750-a44b-47a4-8ce8-9979ab939444" />
</div>

4. 순서
<div align="center">
<img src="https://github.com/user-attachments/assets/1ee118ac-c31c-43a5-a0ae-7f78cdfb0c84" />
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/1e3ed177-adb3-4bbf-b4f9-30fbfd31167b" />
</div>

   - VPC 생성 : Public / Private Subent 각 2개
   - RDS 서브넷 그룹 생성 : RDS 프로비전
   - EFS 생성
   - S3 버킷 생성 : wp-config 업데이트 및 업로드
   - 유저 데이터 준비 및 EC2 생성 : 해당 EC2로 WordPress 생성 - AMI
   - 해당 AMI로 Launch Template : AutoScaling Group + ALB
   - CORS / 보안그룹으로 인해 최초 설치 경로를 바라보는 파일 로딩 불가능 : 직접 로그인해서 WordPress 주소 및 경로 수정 필요
   - 모든 검증이 끝나면 리소스 삭제
