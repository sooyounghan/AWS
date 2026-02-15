-----
### Resource
-----
1. '무엇을'에 해당하는 내용 정의
2. ARN을 사용해 리소스 표현 (예) ```"arn:aws:dynamodb:us-east-2:12345678:table/books_table"```)
3. 리스트 및 와일드카드로 여러 리소스 표현 가능
4. Variable 사용 가능 (예) ```arn:aws:dynamodb:us-east-2:account-id:table/${aws:username}"```)

-----
### NotResource
-----
1. '이것을 제외하고'에 해당하는 내용들 정의
2. 예) 특정 버킷을 제외한 모든 버킷에 대해 ~
