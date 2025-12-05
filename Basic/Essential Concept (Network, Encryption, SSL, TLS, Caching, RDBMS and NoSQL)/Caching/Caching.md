-----
### 캐싱 (Caching)
-----
1. 데이터 복제본 혹은 연산 결과를 임시로 저장하여 요청의 응답을 효율적으로 하는 기술 : 자주 사용되는 복잡한 연산의 결과 혹은 자주 찾는 데이터를 효율적으로 전달하는 것이 목적
2. 장점
   - 요청에 따라 빠르게 데이터 전달 가능
   - 복잡한 리소스 / 부하를 줄일 수 있음
3. 단점
   - 일관성 유지가 어려움
   - 아키텍쳐 복잡도 증가
   - 비용 증가
<div align="center">
<img src="https://github.com/user-attachments/assets/8020c233-1faf-4ad7-9a58-2f89efdd2349" />
<img src="https://github.com/user-attachments/assets/3555e535-e9ee-4641-b9ff-09f6fbef0b79" />
<img src="https://github.com/user-attachments/assets/70e82927-e05d-4b8b-ac9a-e8fd4260cb73" />
</div>

4. 주요 개념
   - 원본 : 캐싱할 데이터 혹은 연산 결과를 제공하는 주체
   - Cache Hit : 요청에 따라 캐시에 저장된 데이터로 응답할 수 있는 상황
     + 캐싱에서 지향할 상황이며 원본에 요청 없이 응답 가능
   - Cache Miss : 요청에 따른 데이터를 캐시에서 찾을 수 없는 상황
     + 별도로 원본에 요청 혹은 다른 방식으로 응답 필요
   - 캐시 만료(Invaildation / Eviction) : 캐시를 삭제하는 행위
     + TTL (Time To Live) : 캐시가 얼마만큼 살아있는지를 나타내는 시간 단위

5. 캐시 방식 : 캐시에 원본 데이터를 채우는 다양한 정책
   - Lazy Loading : 요청이 있을 때만 캐시에 원본 데이터르 채우는 정책 (불필요 요청이 없으나 최초 데이터 로딩 필요)
<div align="center">
<img src="https://github.com/user-attachments/assets/fb58e63b-1c21-49ad-89be-349cf55e6535" />
</div>

   - Eager Loading : 미리 캐시에 데이터를 채워두고 요청을 기다리는 정책 (데이터가 항상 준비되어 있으나 모든 데이터를 채우기에 불필요한 캐시 용량이 낭비될 수 있고, 최초 로딩이 매우 큰 리소스가 필요함)
<div align="center">
<img src="https://github.com/user-attachments/assets/e9669b4f-7e2d-4233-865e-690a51784476" />
</div>

   - Write Through : 데이터가 변경되거나 저장되는 시점에 원본과 캐시에 동시에 저장 (데이터 일관성이 항상 보장되지만, 쓰기 과정이 복잡해지고 느려질 수 있음)
<div align="center">
<img src="https://github.com/user-attachments/assets/b1a8e765-964f-42e6-8adb-6cc071d9b3f6" />
</div>

   - Write Back : 데이터를 캐시에 먼저 쓰고, 후에 원본에 업데이트 (쓰기가 빠르고 간단하지만 원본에 도달 전 유실될 가능성이 있음)
<div align="center">
<img src="https://github.com/user-attachments/assets/a1455179-5da0-40e9-895d-5499bcb86e4c" />
</div>

6. 캐시 만료 방식
   - TTL (Time To Live) : 일정 시간이 지나면 만료 (항상 일정 시간 후 데이터가 만료되고 갱신할 수 있으나 모든 데이터를 항상 갱신 필요)
   - Least Recently Used : 가장 사용 시점이 먼 캐싱 데이터부터 만료 (사용이 발생한 데이터들을 계속 보관할 수 있으나, 상황에 따라 비효율적일 수 있음)
   - Least Frequently Used : 가장 덜 사용된 데이터부터 만료 (자주 사용된 데이터를 남겨둘 수 있으나 현재 자주 사용되지 않는 데이터도 남겨둘 수 있음)
   - First In First Out : 먼저 들어온 데이터부터 만료 (간단하지만 패턴을 고려하지 않아 비효율적)

7. 사용 사례
   - Content Delivery Network (CDN)
   - ElasticCahce (Redis / Memcached) : 랭킹, 세션 데이터 등
   - 웹 브라우저
   - RAM
   - DNS
   
