-----
### Action, NotAction
-----
1. Action
   - 특정 Allow / Deny 할 Action 정의
   - 예) s3.GetObject
   - 와일드카드로 모든 Action 표현 가능 (예) ```e2:*```)
   - LIST로 설정 가능 (예) ```["s3.*", "ec2:*"]```

2. NotAction
   - 지정한 Action 이외에 해당하는 조건 정의
   - 예
```json
"Effect": "Allow",
"NotAction": "s3.DeleteBucket",
"Resource": "arn:aws:s3:::*"
```
<div align="center">
<img src="https://github.com/user-attachments/assets/15ea7407-76cf-410c-8e73-f74342ce0a2c" />
</div>
