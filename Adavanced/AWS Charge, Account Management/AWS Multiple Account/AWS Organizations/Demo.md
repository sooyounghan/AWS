-----
### Demo - Organizations에서 신규 계정 생성
-----
1. 신규 계정 생성
   - AWS Organizations - 조직 생성 - AWS 계정 추가 - aws-lecture-sandbox / GMail로 여러 보조 이메일 활용해 계정 소유의 이메일 주소 작성 - IAM 계정 생성
   - 새로운 계정으로 이동 : 해당 계정 ID를 복사한 후, 역할 전환 - Account ID에 해당 계정 ID 작성 / IAM Role Name에는 OrganizationAccountAccessRole / Display Name : aws-lecture-sandbox
    
2. SCP 테스트
   - 멀티 세션 지원 켜기 - 세션 추가 - aws-lecture-sandbox
   - 관리 계정 : aws-lecture-sandbox 선택 후 정책 - 사용 가능한 7개의 정책 클릭 - 서비스 제어 정책 활성화 - 정책 생성 - 정책 이름 : deny-ec2-grater-than-xlarge, 정책 설명에 아래 코드 붙여넣기 정책 생성
     + 정책 - 연결 - deny-ec2-grater-than-xlarge - 정책 연결
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "limitedSize",
            "Effect": "Deny",
            "Action": "ec2:RunInstances",
            "Resource": "arn:aws:ec2:*:*:instance/*",
            "Condition": {
                "ForAnyValue:StringNotLike": {
                    "ec2:InstanceType": [
                        "*.nano",
                        "*.small",
                        "*.micro",
                        "*.medium",
                        "*.large",
                        "*.xlarge"
                    ]
                }
            }
        }
    ]
}
```
   - 새로운 계정 : EC2 인스턴스 생성 - demo-ec2 / 키 페어 없이 계속 진행 / x2.xlarge 생성 시 오류 발생
     + SCP에 의해 Deny됨을 알 수 있음

   - SCP 분리 후 실시 (정책 - deny-ec2-grater-than-xlarge - 분리) : 새로운 계정에서 EC2 인스턴스 생성 - demo-ec2 / 키 페어 없이 계속 진행 / x2.xlarge 생성 가능
   - 정책 다시 연결 후, 다른 EC2 생성 확인 (x2.medium)
   - EC2 정리

3. GMail로 여러 보조 이메일 만들기
   - GMail 아이디에 ```+```를 넣어 보조 이메일 생성 가능
   - 예시) 이메일 주소 : ```spark@rubywave.io```
     + 보조 이메일
       * ```spark+awsyoutube@rubywave.io```
       * ```spark+test@rubywave.io```
     + AWS Organization 계정 생성 시 활용 가능
