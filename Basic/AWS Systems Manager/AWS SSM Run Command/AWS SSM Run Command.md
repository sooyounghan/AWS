-----
### AWS SSM Run Command
-----
1. 관리중인 노드에게 원격에서 명령을 실행할 수 있는 서비스 (관리중인 노드 : SSM Agent가 설치된 상태에서 SSM의 관리를 받는 EC2 인스턴스 / On-Premise 서버)
2. 주로 단발성 명령을 수행할 때 활용 (예) 관리 중 서버 전체에 신규 서비스 설치, 특정 서비스 혹은 애플리케이션 재 시작, 로그 파일 캡쳐 등)
3. 미리 준비된 문서(Document)에 지정된 명령 수행 가능 (문서 : AWS에서 제공하거나 유저가 직접 작성한 명령어 모음) - 버전 관리 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/d8564aad-a2b5-4116-a31a-bae0c84fb2d5" />
</div>

4. 태그 기반 / 대상 이름 / 인스턴스 ID 기반으로 대상 선정 : 다수의 인스턴스 클러스터에 명령 수행 가능
   - 단, Eventual Consistency 지향 : Async하게 명령 처리

-----
### Demo - Run Command를 활용한 버전 업데이트
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/3851fe62-0b4c-4042-b6e9-3d0d1a8f5751" />
</div>

1. EC2 인스턴스 프로비전 후, Run Command로 버전 업데이트
    - IAM 역할 생성 : EC2 (EC2 Role for AWS Systems Manager)
      + 역할 이름 : demo-ec2-role-for-ssm
    - EC2 인스턴스 2개 프로비전 (인스턴스 개수 2개 설정)
      + demo-ec2-instance
      + t2.micro / 키 페어 없이 진행 / 보안그룹 : default
      + IAM 인스턴스 프로파일 : demo-ec2-role-for-ssm
      + 유저 데이터
```
#!/bin/bash
sudo -s
dnf install httpd -y
service httpd start
chkconfig httpd on
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
echo "$INSTANCE_ID" >> /var/www/html/index.html
```

   - Systems Manager - 공유 리소스 - 문서로 참조 가능
   - 문서 생성 - 명령 또는 세션
     + 문서 이름 : demo-update-index-html
     + 콘텐츠 작성 (JSON)
```json
{
  "schemaVersion": "2.2",
  "description": "Update the version in the index.html file and restart the Apache web server.",
  "parameters": {
    "version": {
      "type": "String",
      "description": "The version to update in the index.html file.",
      "default": "1.0.0"
    }
  },
  "mainSteps": [
    {
      "action": "aws:runShellScript",
      "name": "updateVersionInIndex",
      "inputs": {
        "runCommand": [
          "#!/bin/bash",
          "if grep -q 'Version:' /var/www/html/index.html; then",
          "    sed -i 's/Version:.*/Version: {{ version }}/' /var/www/html/index.html",
          "else",
          "    echo 'Version: {{ version }}' >> /var/www/html/index.html",
          "fi",
          "service httpd restart"
        ]
      }
    }
  ]
}
```
   - 내 소유를 누르면 확인 가능
   - 세부 정보 - 파라미터 입력 가능 (Version)
   - Systems Manager - 인벤토리 - 해당 관리형 인스턴스 2개 생성 확인

2. EC2 안 index.html을 Run Command 명령으로 수정 : 수정 시, Run Command 내에 입력한 버전을 추가 혹은 업데이트
   - Systems Manager - demo-update-index-html에서 명령 실행
     + 문서 버전 : 최신값
     + Version : 1.0.3
     + 수동으로 인스턴스 2개 선택
     + 속도 제어 : 얼마나 많은 인스턴스를 동시에 적용시킬 것인지 결정 / 오류 허용 정도
     + 출력 옵션 : 명령에 대한 결과에 대해 CloudWatch나 S3에 입력 가능
     + 알람 및 SNS 받기 가능
     + EC2 인스턴스에 접근해서 EC2 인스턴스 안에서 입력하고 싶을 때, 사용할 CLI 명령 제공

    - EC2 인스턴스 Public DNS로 접속하면 {인스턴스ID}-버전 정보 출력
    - 문서 - 내 소유 - 명령 실행 - Version : 1.3으로 변경 후 재실행하면 Version 값이 변경됨 확인 가능

3. 리소스 정리 : EC2 정리


