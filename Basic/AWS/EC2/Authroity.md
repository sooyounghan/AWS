-----
### EC2 권한 부여
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/a09fa5a3-697c-4dce-b719-109f7984ad84" />
</div>

1. IAM 자격 증명 등록
   - IAM 사용자를 생성하고 IAM 자격증명을 발급받아 EC2에 등록
   - AWS Configure를 통해 자격 증명을 파일로 등록 (./aws/credentials)
   - 관리가 어렵고 바꾸기 힘듬 (예) EC2 100대의 자격 증명의 교체할 경우)

2. IAM 역할 부여
   - 권한이 부여된 IAM 역할을 만들고 EC2에 부여
   - 관리가 쉽고 교체가 쉬움
   - 내부적으로 지속적으로 자격증명 변경 : 뛰어난 보완성

3. 역할 (Role)
<div align="center">
<img src="https://github.com/user-attachments/assets/54cc4659-2a86-468b-9241-4317a9d31024" />
<img src="https://github.com/user-attachments/assets/77d439a7-c9f1-40df-94a4-cc084abf9973" />
<img src="https://github.com/user-attachments/assets/0531755b-d957-4fbf-b088-b67680b71c29" />
<img src="https://github.com/user-attachments/assets/84e2c027-4b50-43ce-b46f-4862832ffd91" />
</div>

4. 자격 증명 교체
<div align="center">
<img src="https://github.com/user-attachments/assets/d845ca4d-00d4-4023-9f13-bb77c65c7cbc" />
<img src="https://github.com/user-attachments/assets/c3a05ca6-4dc0-4126-ae7b-80fd247260d9" />
</div>

5. 실습 - AWS EC2 권한 부여
   - IAM 역할 생성
     + EC2가 사용할 역할 : 좌측 액세스 관리 - 역할 - 역할 생성 - AWS 서비스 / 사용 사례 : EC2 선택
     + 권한 정책 : IAMReadOnlyAccess
     + 역할이름 : demo-iam-role-for-ec2    
   - EC2 생성 후 역할 연동 : 인스턴스 프로파일로 IAM과 연동
     + 이름 : demo-iam-role
     + 키 페어 사용
     + 보안 그룹 : 다음에서 SSH 트래픽 허용 / 인터넷에서 HTTP 트래픽 허용 선택
     + 고급 세부 정보 : IAM 인스턴스 프로파일 (demo-iam-role-for-ec2) 선택
     + 사용자 데이터
```bash
#!/bin/bash
curl --silent --location https://rpm.nodesource.com/setup_20.x | bash -
dnf -y install nodejs
```

   - AWS CLI를 활용해 IAM 사용자 목록 출력
     + sudo -s
     + aws iam list-users
```bash
[root@ip-172-31-34-32 ec2-user]# aws iam list-users
{
    "Users": [
        {
            "Path": "/",
            "UserName": "admin",
            "UserId": "...",
            "Arn": "arn:aws:iam::362454057659:user/admin",
            "CreateDate": "2025-12-06T09:08:48+00:00",
            "PasswordLastUsed": "2025-12-10T03:22:34+00:00"
        },
        {
            "Path": "/",
            "UserName": "demo-iam-ec2-user",
            "UserId": "...",
            "Arn": "arn:aws:iam::362454057659:user/demo-iam-ec2-user",
            "CreateDate": "2025-12-07T07:05:51+00:00"
        },
        {
            "Path": "/",
            "UserName": "demo-iam-user",
            "UserId": "...",
            "Arn": "arn:aws:iam::362454057659:user/demo-iam-user",
            "CreateDate": "2025-12-07T06:46:18+00:00"
        }
    ]
}`
```
   - 별도로 설정하지 않아도 Access Key와 Secret Access Key를 입력하지 않았지만, IAM 역할을 통해 권한 부여했기에 가능
      + demo-iam-role-for-ec2 역할 삭제 후 재확인 : 권한이 없다고 출력 
```bash
[root@ip-172-31-34-32 ec2-user]# aws iam list-users

An error occurred (AccessDenied) when calling the ListUsers operation: User: arn:aws:sts::362454057659:assumed-role/demo-iam-role-for-ec2/i-0b03f7c50e9943963 is not authorized to perform: iam:ListUsers on resource: arn:aws:iam::362454057659:user/ because no identity-based policy allows the iam:ListUsers action
```

   - Node.js 애플리케이션에서 IAM 사용자 목록 출력
     + test.js
```js
// AWS SDK v3에서 IAM 클라이언트 클래스를 가져옴
const { IAMClient, ListUsersCommand, ListRolesCommand } = require("@aws-sdk/client-iam");

// AWS SDK v3 클라이언트는 모듈화 : 각 서비스마다 자신의 클라이언트 모듈 존재

async function runTest() {
    const client = new IAMClient({
        region: "ap-northeast-2", // 리전 설정

    });
    // 사용자 목록
    console.log("사용자 목록 출력");
    try {
        const dataUser = await client.send(new ListUsersCommand({}));
        dataUser.Users.forEach((element) => {
            console.log(element.UserName); // 사용자 이름 출력
        });
    } catch (error) {
        console.error(error); // 오류 처리
    }
}

runTest();
```
   - fileZilla를 이용해 demo-aws-credential 압축 파일을 EC2로 전송 후 EC2에서 압축 해제 : unzip demo-aws-credential.zip
     + cd src
     + dir
     + cd lambda/
     + dir
     + nano test.js : Access Key와 Secret Access Key가 없는 상태
     + node ./test.js : 정상적 실행
       * EC2 인스턴스에 부여받은 IAM 역할이 안에 있는 모든 애플리케이션들에게 권한을 부여한 것
       * 권한을 제거하면, 권한이 없다고 오류 발생
       * 💡 따라서, EC2에서 애플리케이션을 올려서 사용할 때는 Access Key와 Public Access Key를 사용하기보다 IAM 역할 사용
```bash
[root@ip-172-31-34-32 lambda]# node ./test.js
사용자 목록 출력
admin
demo-iam-ec2-user
demo-iam-user
```
