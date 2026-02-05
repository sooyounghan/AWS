-----
### Amazon SQS의 Queue 타입
-----
1. Standard
   - Standard SQS는 순서를 보장하지 않음
   - Standard SQS는 메세지를 여러 번 전달할 가능성 존재 : At Least Once Delivery

2. FIFO
   - 순서 보장 및 메세지를 단 한 번만 전달하도록 보장하나 성능 제약
   - Exacctly Once Delivery

3. 즉, Standard 모드를 사용할 때 멱등성(Idempotency) 확보 필요
   - 멱등성(Idempotency) : 로직이 여러 번 수행되더라도 동일한 결과를 보장할 수 있는 성질
   - 예) 한 자리에 대한 기차표가 한 대상에게 여러 번 구매되더라도 요금이 여러 번 결제되지 않는 것

