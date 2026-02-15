-----
### AWS Variable
-----
1. aws:CurrentTime : 현재 시간 비교
2. aws:EpochTime : 현재 유닉스 타임
3. aws:TokenIssueTime : 임시 자격 증명에서 토큰이 발급된 시간 (Assume 된 시간)
4. aws:PrincipalType : 권한 행사 주체 타입 (Account, User, FederatedUser, AssumedRole)
5. aws:SecureTransport : SSL 통신으로 권한이 행사중인 확인
6. aws:SourceIp : Source IP
7. aws:UserAgent : 유저 에이전트 정보 (주의 : 변조 가능)
8. aws:userid : IAM Unique Identifiers
9. aws:username : AWS 유저 이름
10. ec2:SourceInstanceARN : IAM Role을 사용해서 EC2에서 요청을 한 경우에만 사용 (Source Instance ARN)
