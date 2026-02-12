-----
### CloudFront의 파일 관리
-----
1. 싱글 파일 : 파일명을 유지한 채로 캐시 만료 처리, 업데이트 등 처리
   - 별도로 클라이언트 업데이트 필요 없음
   - 캐시 만료 전 제공 파일을 업데이트 하려면 Invalidation 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/75e25b3c-7c7c-4614-b7b2-ea7f2ed88665" />
</div>

2. 버저닝 : 파일 이름에 다양한 방법으로 버전을 두어서 관리
   - 별도로 Invalidation 필요 없음
   - 파일 업데이트 시 클라이언트 업데이트 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/cbb929e6-5240-4af2-af72-10e84953b8eb" />
<img src="https://github.com/user-attachments/assets/b01ea95a-f20e-46c4-b12e-f52609c7bae6" />
<img src="https://github.com/user-attachments/assets/a5d87e22-c2be-4dec-bff1-597eb180135f" />
</div>

-----
### Invalidation
-----
1. 캐시 만료 전 파일을 갱신
2. 버저닝이 아닌 형태로 파일 제공할 경우, 캐시 만료 전 새 파일을 제공하고 싶다면, Invalidation 필요
3. 경로 기반
    - 예시) ```/img/img1.png, /img/*, /img/img*```
4. 한 번에 최대 3000파일까지 Invalidate 가능 (예) 100개씩 30 Invalidations 또는 1000개씩 3 Validations)
5. 한 달에 1000 Path Invalidations은 무료 (계정 전체 Distribution 통합), 이후 한 번 경로당 $0.005
