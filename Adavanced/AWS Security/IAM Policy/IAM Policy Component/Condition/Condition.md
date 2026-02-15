-----
### Condition
-----
1. IAM Policy 정책의 적용 조건을 판별 가능

2. Condition - String
<div align="center">
<img src="https://github.com/user-attachments/assets/a193d415-1b81-4048-ba3e-1faaf7f9ecbe" />
</div>

   - 예) S3 버킷 중 Path가 marketing/... 일 경우 : "Condition": {"StringLike": {"s3:prefix": "marketing/*"}}

3. WildCard
   - ```*``` : 길이 제한 없는 Wildcard
     + 예) ```"Resource": "arn:aws:s3:::*-bucket*"```
       *  ```arn:aws:s3:::123456789-bucketfortest```

   - ```?``` : 길이 한글자 Wildcard
     + 예) ```"Resource": "arn:aws:dynamodb:*:*:table/test?table"```
       * ```arn:aws:dynamodb:us-east-1:123456789:table/test_table```
       * ```arn:aws:dynamodb:us-east-1:123456789:table/test-table```

4. Condition - Numeric
<div align="center">
<img src="https://github.com/user-attachments/assets/569278c2-d2e2-43db-88ff-4274dbc99d45" />
<img src="https://github.com/user-attachments/assets/342b54a2-7c0c-481e-8cc8-d63af747da9a" />
</div>

5. Condition - Date
<div align="center">
<img src="https://github.com/user-attachments/assets/10080271-57f1-454e-8082-a5c48b5978ba" />
<img src="https://github.com/user-attachments/assets/75665d8e-0b9c-4e51-9af3-be40da132b92" />
</div>

6. Condition - 기타
<div align="center">
<img src="https://github.com/user-attachments/assets/c6d91372-1927-49b6-b63e-f5941ac1d8ac" />
</div>

7. Condition - Boolean
<div align="center">
<img src="https://github.com/user-attachments/assets/b996d553-44fa-4e49-85c7-8a023c74b30e" />
</div>

8. Condition - IP Address
<div align="center">
<img src="https://github.com/user-attachments/assets/89af4a29-ae9e-4b9b-8c89-d50860cf3c38" />
</div>

9. Condition - ARN
<div align="center">
<img src="https://github.com/user-attachments/assets/b5145588-e9ee-4c36-8213-19c2854630d8" />
</div>
