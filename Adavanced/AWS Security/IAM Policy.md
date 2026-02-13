-----
### IAM 정책 (Policy)
-----
1. 사용자와 그룹, 역할, AWS 리소스가 무엇을 할 수 있는지에 관한 문서
2. JSON 형식으로 정의
3. 그룹 역할 , 유저, AWS 리소스에 부여되어 각 주체가 행동 가능한 권한 정의
4. 정책의 구성
<img width="582" height="316" alt="image" src="https://github.com/user-attachments/assets/44002f56-8b52-4e0d-923b-67b05d9e3271" />

   - Resources : 어떤 AWS 리소스에 대해서
   - Action : 어떤 행동을
   - Effect : 허용 / 거부
   - Condition : 정책이 허용되는 조건 (예) IP 주소, 시간, 태그 등)
   - Principal : 누가 (리소스 기반 정책에서만 사용)

5. 종류
   - Identity-Based Policies (자격 증명 기반 정책)
     + 자격증명(IAM 유저, 그룹, 역할)에 부여하는 정책
     + 해당 자격 증명이 무엇을 할 수 있는지 정의

   - Resource-Based Policies (리소스 기반 정책)
     + 리소스 (예) S3, SQS, VPC Endpoint, KMS 등)에 부여하는 정책
     + 해당 리소스에 누가, 무엇을 할 수 있는지 정의 (예) SQS 대기열에 Lambda Service가 접근 가능)
       
   - 권한 범위 제한 정책 : 실제로 권한을 주지는 않지만 최대 권한 범위를 제한하는 정책
     + Access Control List
     + 세션 정책(Session Policy) (유저 / 역할이 할 수 있는 역할 제한)
       * 임시자격증명을 생성할 때, 같이 넣어주는 정책
       * 임시자격증명을 권한을 제한하기 위해 활용
<img width="752" height="351" alt="image" src="https://github.com/user-attachments/assets/4478ba03-ecec-4127-982a-880137b91822" />

       * 예) 리소스 기반 정책 : getObject, putObject
         * principal - 유저 / 역할 : putObject, listObject, deleteObject
         * 세션 정책 : getObject, listObject (유저 / 역할과 겹치는 부분인 listObject가 가능)
         * AssumeRole를 통해 세션 키 생성 : 세션 키는 세션 정책이 가지고 있는 범위에서만 가능
       * 즉 세션 정책 : 자격 증명 기반 정책과 리소스 기반 정책의 교집합과 모든 교집합 적용
         * 최종 권한 : getObject[리소스와 세션 정책의 교집합]
         * 최종 권한 : listObject[유저/역할과 세션의 교집합]
         * 💡 putObject : 리소스 기반 정책이 유저 / 역할이 아닌 만들어진 Principal을 넣을 경우 참조하면 사용 가능할 수 있을 때, 사용 가능
<img width="763" height="364" alt="image" src="https://github.com/user-attachments/assets/d05940d0-9fe1-482e-8a45-8c0d51f792e3" />

     + AWS Organizations Service Controls (SCP) : AWS Organizations에서 각 계정의 최대 권한 범위를 제한하는 정책
<img width="724" height="375" alt="image" src="https://github.com/user-attachments/assets/14cfacc0-0e79-44e3-af6a-265a0178cdf0" />

     + 권한 범위 (Permissions Boundaries) : AWS의 사용자나 역할의 권한을 제한하기 위한 정책
<div align="center">
<img width="263" height="243" alt="image" src="https://github.com/user-attachments/assets/4edbe74f-d5c3-4524-88ff-2ab56e3eeb7a" />
</div>

<div align="center">
<img width="412" height="279" alt="image" src="https://github.com/user-attachments/assets/d9c185ab-3d4b-4be0-9f0d-8478c174d7c2" />
<img width="407" height="309" alt="image" src="https://github.com/user-attachments/assets/fbc594ba-4842-400e-8e4b-0d4db00b4074" />
</div>
