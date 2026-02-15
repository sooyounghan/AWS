-----
### 프로그램 방식 액세스 자격 증명 테스트
-----
1. IAM 사용자 생성
  - 사용자 이름 : demo-iam-user (Console Access 권한 주지 않음)
  - 직접 정책 연결 : IAMReadOnlyAccess
 
2. 권한 부여 (정책)
   - 보안 자격 증명 - 액세스 키 - 액세스 키 만들기 - 기타 - 생성하면 액세스 키와 비밀 액세스 키 생성
   - EC2 생성 (Region 설정) - EC2 검색 후 인스턴스 시작
     + 이름 : demo-iam-test
     + 키 페어(로그인) 없이 설정
     + 보안 그룹도 그대로 설정
     + 인스턴스 시작

   - EC2 실행이 완료되면 연결 - 사용자 이름 : ec2-user - 연결
   - 관리자 권한 획득 : sudo -s
     + 현재 AWS CLI, 즉 AWS와 통신할 수 있는 Commad Line이 존재하므로 활용
     + 사용자 목록 출력 : aws iam list-users
```bash
[root@ip-172-31-12-96 ec2-user]# aws iam list-users

Unable to locate credentials. You can configure credentials by running "aws configure".
```
   - Credential이 없으므로 어떤 권한을 가지고 명령을 실행해야 되고, AWS가 통신해야 되는지 알 수 없음
     + aws configure : Access Key와 Secret Acceess Key 입력 후, Default Region 설
     + 다시 aws iam list-users 입력 (IAM 사용자(admin)과 demo-iam-user 사용자 출력)
```bash
[root@ip-172-31-12-96 ec2-user]# aws iam list-users
{
    "Users": [
        {
            "Path": "/",
            "UserName": "admin",
            "UserId": "...",
            "Arn": "arn:aws:iam::362454057659:user/admin",
            "CreateDate": "2025-12-06T09:08:48+00:00",
            "PasswordLastUsed": "2025-12-07T06:43:36+00:00"
        },
        {
            "Path": "/",
            "UserName": "demo-iam-user",
            "UserId": "...",
            "Arn": "arn:aws:iam::362454057659:user/demo-iam-user",
            "CreateDate": "2025-12-07T06:46:18+00:00"
        }
    ]
}
```
   - demo-iam-user 권한 행사 : 보안 자격 증명을 통해 발급받은 액세스 키와 시크릿 액세스 키가 demo-iam-user의 권한을 행사할 수 있게 해줌 (demo-iam-user는 IAM의 모든 권한 조회할 수 있는 IAMReadOnlyAccess 부여)
   - 권한 정책에서 권한 제거 후 재실행하면, 권한 없으므로 오류 발생
```bash
[root@ip-172-31-12-96 ec2-user]# aws iam list-users

An error occurred (AccessDenied) when calling the ListUsers operation: User: arn:aws:iam::362454057659:user/demo-iam-user is not authorized to perform: iam:ListUsers on resource: arn:aws:iam::362454057659:user/ because no identity-based policy allows the iam:ListUsers action
```

   - 보안 자격 증명에서 액세스 키 비활성화 (사용할 수 없는 상태)
```bash
[root@ip-172-31-12-96 ec2-user]# aws iam list-users

An error occurred (AccessDenied) when calling the ListUsers operation: User: arn:aws:iam::362454057659:user/demo-iam-user is not authorized to perform: iam:ListUsers on resource: arn:aws:iam::362454057659:user/ because no identity-based policy allows the iam:ListUsers action
```
   - 활성화하면 다시 IAM 유저 출력

4. Access Key Pair 생성 및 테스트
   - test.js
```js
// AWS SDK v3에서 IAM 클라이언트 클래스를 가져옴
const { IAMClient, ListUsersCommand, ListRolesCommand } = require("@aws-sdk/client-iam");

// AWS SDK v3 클라이언트는 모듈 : 각 서비스마다 자신의 클라이언트 모듈이 존재

async function runTest() {
    const client = new IAMClient({
        region: "ap-northeast-2", // 리전 설정
        // credentials: {
        //     accessKeyId: "...", // 액세스 키 ID
        //     secretAccessKey: "]...", // 비밀 액세스 키
        // }
        // 기본 자격 증명을 사용하는 경우, 자격 증명 객체를 생략할 수 있음
        // SDK는 환경에서 자동으로 자격 증명을 해결
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

   - CLI 및 프로그램(Node.js)에서 사용
```bash
# 실행 방법
1. node16 이상 설치
2. npm install yarn -g
3. yarn
4. node src/lambda/test.js
```

```bash
$ node src/lambda/test.js
사용자 목록 출력
admin
demo-iam-user
```

   - Profile 확인 (CloudShell 사용)
     + demo-iam-ec2-user IAM 사용자 생성 (AmazonEC2ReadOnlyAccess)
     + 보안 자격 증명 (EC2 읽기에 대한 권한을 가짐) - 액세스 키 생성
     + CloudShell에 aws configure --profile iamuser (demo-iam-user)입력 / aws configure --profile ec2user (demo-ec2-user)
       
   - Rotation 및 권한 사용
     + aws iam list-users --profile iamuser
     + aws ec2 describe-instances --profile iamuser (권한이 없음 demo-iam-user는 EC2를 볼 권한이 없으므로, ec2 user로 사용)
     + aws ec2 describe-instances --profile ec2user
```bash
~ $ aws iam list-users --profile iamuser
{
    "Users": [
        {
            "Path": "/",
            "UserName": "admin",
            "UserId": "...",
            "Arn": "arn:aws:iam::362454057659:user/admin",
            "CreateDate": "2025-12-06T09:08:48+00:00",
            "PasswordLastUsed": "2025-12-07T06:43:36+00:00"
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
}
~ $ aws ec2 describe-instances --profile iamuser 
An error occurred (UnauthorizedOperation) when calling the DescribeInstances operation: You are not authorized to perform this operation. User: arn:aws:iam::362454057659:user/demo-iam-user is not authorized to perform: ec2:DescribeInstances because no identity-based policy allows the ec2:DescribeInstances action

~ $ aws ec2 describe-instances --profile ec2user
...
```

   - Configure를 할 때, 한 번에 하나만 쓰면 불편하므로 다양한 Profile을 미리 Configure해놓고, 권한에 맞게 사용 가능
   - 한 계정 뿐만 아니라 다양한 계정을 사용할 때, 다양한 계정별로 프로파일 설정 가능

5. 💡 EC2 삭제 : 인스턴스 상태 - 인스턴스 종료(삭제)
