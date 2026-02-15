-----
### Amazon RedShift
-----
1. 클라우드에서 완벽하게 관리되는 페타바이트급 데이터 웨어하우스 서비스
2. 작게는 수백 기가바이트부터 시작하여 페타바이트 이상까지 데이터 확장 가능
3. 이를 통해 데이터를 사용하여 비즈니스 및 고객에 대한 새로운 인사이트를 발굴하는 것이 가능
4. 즉, AWS에서 제공하는 Data Warehouse 서비스 = OLAP 데이터베이스
   - OLAP와 OLTP는 서로 다른 아키텍쳐 및 시스템 구조를 요구함
   - 기존의 데이터베이스와 비교하여 OLAP 데이터에 최적화된 압축 방식 적용
5. 병렬 처리에 탁월한 성능 : 컴퓨팅 노드에 증감이 쉽고 빠름
6. 구조
   - Leader Node : 하나의 Leader Node에 다수의 Compute Node를 통솔하며 쿼리 방식 / 순서 / 작업 등을 계획하고 분배
   - Compute Node : 실제 연산을 담당하는 Node로 다양한 Type(CPU, RAM 등의 구성의 집합)으로 구성
   - Managed Storage : 실제 데이터를 저장하는 공간으로 Amazon S3 활용
   - 기타 : Connector, Data API, 네트워크 등
<div align="center">
<img src="https://github.com/user-attachments/assets/9c303c22-a29d-4969-8e11-ac60b8938c4b" />
</div>


7. 기타 기능
   - Redshift Spectrum : 정형화된 S3의 데이터를 Redshift에 데이터 로딩 없이 쿼리할 수 있는 서비스 (Redshift의 강력한 쿼리 및 능력만을 활용)
   - Redshift Serverless : 별도로 인프라의 프로비전 및 확장을 고민하지 않고 Redshift를 활용하는 모드 (자동 스케일링을 지원하고 사용한 만큼만 과금)
   - Federated Querires : 다른 데이터베이스(예) Amazon Aurora)의 내용을 직접 쿼리하고 분석하는 기능
   - 기타
     + Machine Learning 등을 활용하여 예측
     + CloudWatch / CloudTrail을 활용한 모니터링 및 감사
     
