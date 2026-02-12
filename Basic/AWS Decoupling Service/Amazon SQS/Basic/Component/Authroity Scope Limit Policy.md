-----
### 권한 범위 제한 정책
-----
1. 실제로 권한을 주지는 않지만 최대 권한 범위를 제한하는 정책
2. 종류
   - 세션 정책(Session Policy) (유저 / 역할이 할 수 있는 역할 제한)
     + 임시자격증명을 생성할 때, 같이 넣어주는 정책
     + 임시자격증명을 권한을 제한하기 위해 활용
     + 예) 리소스 기반 정책 : getObject, putObject
       * principal - 유저 / 역할 : putObject, listObject, deleteObject
       * 세션 정책 : getObject, listObject (유저 / 역할과 겹치는 부분인 listObject가 가능)
       * AssumeRole를 통해 세션 키 생성 : 세션 키는 세션 정책이 가지고 있는 범위에서만 가능
       * 즉 세션 정책 : 자격 증명 기반 정책과 리소스 기반 정책의 교집합과 모든 교집합 적용 (최종 권한 : getObject[리소스와 세션], listObject[유저/역할과 세션], putObject[리소스 기반 정책이 유저 / 역할이 아닌 만들어진 Principal을 넣을 경우 참조하면 사용 가능할 수 있음] 사용 가능)

   - AWS Organizations Service Controls (SCP) : AWS Organizations에서 각 계정의 최대 권한 범위를 제한하는 정책
   - 권한 범위 (Permissions Boundaries) : AWS의 사용자나 역할의 권한을 제한하기 위한 정책
