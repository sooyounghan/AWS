-----
### AWS SSM Run Command
-----
1. 관리중인 노드에게 원격에서 명령을 실행할 수 있는 서비스 (관리중인 노드 : SSM Agent가 설치된 상태에서 SSM의 관리를 받는 EC2 인스턴스 / On-Premise 서버)
2. 주로 단발성 명령을 수행할 때 활용 (예) 관리 중 서버 전체에 신규 서비스 설치, 특정 서비스 혹은 애플리케이션 재 시작, 로그 파일 캡쳐 등)
3. 미리 준비된 문서(Document)에 지정된 명령 수행 가능 (문서 : AWS에서 제공하거나 유저가 직접 작성한 명령어 모음)
<div align="center">
<img src="https://github.com/user-attachments/assets/d8564aad-a2b5-4116-a31a-bae0c84fb2d5" />
</div>

4. 태그 기반 / 대상 이름 / 인스턴스 ID 기반으로 대상 선정 : 다수의 인스턴스 클러스터에 명령 수행 가능
   - 단, Eventual Consistency 지향 : Async하게 명령 처리

-----
### Demo - Run Command를 활용한 버전 업데이트
-----
1. EC2 인스턴스 프로비전 후, Run Command로 버전 업데이트
2. EC2 안 index.html을 Run Command 명령으로 수정 : 수정 시, Run Command 내에 입력한 버전을 추가 혹은 업데이트
