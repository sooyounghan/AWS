-----
### RDS Proxy 사용
-----
1. RDS Provision : Secrets Manger 활용 (비용 조금 발생)
   - RDS - 데이터베이스 생성 : MySQL (프리티어), my-db, 자체 증명 관리 : AWS Secrets Manager에서 관리
   - 퍼블릭 액세스 허용
   - EC2 권한 생성 (IAM)
     + 역할 - 역할 생성 : EC2, SecretsManagerReadWrite - demo-ec2-role-for-rds
     + 추가 권한 부여 : 권한 - 권한 추가 - 인라인 정책 생성 - JSON (RDS DB Connect를 IAM 인증을 통해 사용 - RDS Proxy에서 사용)
     + 이름 : rds-allow 
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "rds-db:connect"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

   - Secrets Manager
     + 보안 암호 이름에 ! 존재하는 이름 선택하면, 보안 암호 검색 가능 (보안 암호 - 보안 암호 값)

   - EC2 생성
     + demo-rds-connect
     + my-db-keypair 사용
     + 기존 보안 그룹 선택 : default
     + IAM 인스턴스 프로파일 : demo-ec2-role-for-rds
     + 유저 데이터에 Node.js 스크립트 파일 지정
```
#!/bin/bash
curl --silent --location https://rpm.nodesource.com/setup_20.x | bash -
dnf -y install nodejs
```

2. RDS Proxy 설정
   - RDS - 프록시 - 프록시 생성 - MariaDB 및 MySQL (식별자 : my-db-proxy, 대상 그룹 구성의 데이터베이스 : my-db)
   - 인증 - IAM 역할 : 기본 역할 생성
   - Secrets Manager는 생성한 것 사용 
   - IAM 인증 : 허용되지 않음 먼저 실시
   - 연결 - 전송 계층 보안 필요 활성화

3. Node.js 기반 애플리케이션 접속 테스트
   - 소스 코드 : demo-secrets-manager
     + .env의 secret_id : 생성한 아이디
     + .env의 host : RDS 엔드포인트
     + .env의 rds_proxy_host : RDS Proxy 엔드포인트
     + 변경 후 해당 폴더 압축 해 FileZila로 전송 (EC2의 Public IPv4 DNS로 전송)
       * 사이트 관리자 호스트에 입력 후, 사용자는 ec2-user, 키 파일은 생성했던 키 페어 파일
   - EC2 연결
```
sudo -s
unzip demo-secrets-manager.zip
npm install
```

   - secrets_test.js : Secrets Manager에서 Secret을 가져온 뒤, 이를 기반으로 데이터 베이스 연결을 수립하고 쿼리를 전송
```js

import dotenv from 'dotenv';

import { Signer } from '@aws-sdk/rds-signer';
import mysql from 'mysql2/promise';
import { SecretsManagerClient, GetSecretValueCommand } from '@aws-sdk/client-secrets-manager';



dotenv.config();
async function runTest() {
    try {
        const client = new SecretsManagerClient({ region: process.env.region });
        const command = new GetSecretValueCommand({ SecretId: process.env.secret_id }); // 시크릿 ID 가져옴
        const res = await client.send(command);
        const secret = JSON.parse(res.SecretString);
        let dbinfo = { 
            connectionLimit: 100,
            host: process.env.host,
            user: secret?.username,
            password: secret?.password,
            port: 3306,

            timezone: "+09:00",
        }
        console.log(dbinfo);
        let connection = undefined;
        connection = await mysql.createConnection(dbinfo);
        const [rows] = await connection.query("select now()"); // 쿼리
        console.log(JSON.stringify(rows));
        connection.end();
    }
    catch (error) {
        console.error('Error retrieving secret:', error);
        throw error;
    }
}

runTest();
```
   - proxy_test.js
```js

import dotenv from 'dotenv';

import { Signer } from '@aws-sdk/rds-signer';
import mysql from 'mysql2/promise';
import { SecretsManagerClient, GetSecretValueCommand } from '@aws-sdk/client-secrets-manager';

dotenv.config();
async function runTest() {
    try {
        const client = new SecretsManagerClient({ region: process.env.region });
        const command = new GetSecretValueCommand({ SecretId: process.env.secret_id }); 
        const res = await client.send(command);
        const secret = JSON.parse(res.SecretString);
        let dbinfo = {
            connectionLimit: 100,
            host: process.env.rds_proxy_host, // 프록시로 접근
            user: secret?.username,
            password: secret?.password,
            port: 3306,
            ssl: { rejectUnauthorized: true },
            timezone: "+09:00",
        }
        console.log(dbinfo);
        let connection = undefined;
        connection = await mysql.createConnection(dbinfo);
        const [rows] = await connection.query("select now()");
        console.log(JSON.stringify(rows));
        connection.end();
    }
    catch (error) {
        console.error('Error retrieving secret:', error);
        throw error;
    }
}

runTest();
```
   - iam_auth_test.js : 실행 하기전 Proxy 수정 - IAM 인증 : 필수 변경 필요
```js

import dotenv from 'dotenv';

import { Signer } from '@aws-sdk/rds-signer';
import mysql from 'mysql2/promise';
import { SecretsManagerClient, GetSecretValueCommand } from '@aws-sdk/client-secrets-manager';


dotenv.config();
async function runTest() {
    try {
        const signer = new Signer({
            region: process.env.region,
            hostname: process.env.rds_proxy_host,
            port: 3306,
            username: process.env.username,
        });
        const token = await signer.getAuthToken();

        let dbinfo = {
            connectionLimit: 100,
            host: process.env.rds_proxy_host,
            user: process.env.username,
            password: token,
            port: 3306,
            ssl: { rejectUnauthorized: true },
            timezone: "+09:00",
        }
        console.log(dbinfo);
        let connection = undefined;
        connection = await mysql.createConnection(dbinfo);
        const [rows] = await connection.query("select now()");
        console.log(JSON.stringify(rows));
        connection.end();
    }
    catch (error) {
        console.error('Error retrieving secret:', error);
        throw error;
    }
}

runTest();
```
   - Secrets Manager를 활용해 인증 정보를 받아와 RDS와 통신
   - RDS Proxy를 사용해 RDS와 통신
   - RDS Proxy + IAM 인증을 사용해 RDS와 통신
```
cd src
node secret_test.js
node proxy_test.js
node iam_auth_test.js
```

4. 리소스 정리
   - EC2 인스턴스 종료
   - RDS Proxy 종료 및 RDS 삭제
   - Secrets Manager 삭제 (7일 이후 완전히 삭제)
   
