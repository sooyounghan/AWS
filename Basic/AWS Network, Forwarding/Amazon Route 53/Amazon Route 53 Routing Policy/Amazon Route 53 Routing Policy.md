-----
### Amazon Route 53 Routing Policy
-----
1. 도메인 레코드에 대상을 연결하는 방식을 다양하는 방법으로 지원
2. 종류
   - Simple
   - Failover
   - Geolocation
   - Geoproximity
   - Latency
   - IP-Based
   - Multivalue Answer
   - Weighted

-----
### Simple Routing Policy
-----
1. 가장 간단한 방식으로 하나의 레코드를 하나의 대상으로 라우팅하는 방식
2. 단일 서버, 단일 리소스 등의 라우팅을 위해 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/7edced5e-bfbf-422e-8414-c80c24cd53d6" />
</div>

-----
### Failover Routing Policy
-----
1. 평소에는 기본 대상(Primary)으로 라우팅하고, 기본 대상에 문제가 있을 때 보조 대상(Secondary)으로 라우팅하는 정책
2. Health Check을 활용하여 상태 확인 : 기본 대상의 Health Check이 Fail 상태일 경우, 보조 대상으로 라우팅
3. 주용 사용 사례
   - Active-Passive Failover
     + 기본 대상이 대부분의 요청을 처리하고, 기본 대상의 실패를 대비해 보조 대상이 준비만 하는 Failover 방식
     + 즉, 평소에 대부분의 리소스를 기본 대상에 집중하고, 보조 대상을 최소한 리소스로 장애를 준비
<div align="center">
<img src="https://github.com/user-attachments/assets/279e1deb-42d9-41ff-8e51-5ffe5b4bdd71" />
<img src="https://github.com/user-attachments/assets/b52793e7-3b88-46c7-91ec-63babd613d51" />
</div>

-----
### Geolocation Routing Policy
-----
1. DNS Query가 발생한 위치에 따라 다른 응답을 보낼 수 있는 Routing Policy
   - 즉, 요청한 지점이 지리적으로 어디에 위치해 있는지에 따라 응답을 다르게 설정 가능
   - 예) 동남아시아 지역에서 발생한 요청은 한국 리전, 유럽 지역에서 발생한 요청은 프랑크푸르트 리전

2. 대륙, 나라 + 미국 일 경우, 주(State) 기준으로 설정 가능 : 겹치는 범위라면 더 작은 지역을 우선 적용
3. 💡 내가 지정한 지역의 요청은 내가 지정한 지역으로 라우팅
4. 주요 사용 사례
   - 언어 별 라우팅
   - 지역별 컨텐츠 제공 구분
   - 예상 가능한 부하를 기반(인구 등)으로 인프라를 구축하고 유지
<div align="center">
<img src="https://github.com/user-attachments/assets/4c5c0cc5-5089-40f0-8640-d99fb3f44a7b" />
</div>

-----
### Geoproximity Routing Policy
-----
1. DNS Query가 발생한 위치와 리소스 위치에 따라 다른 응답을 보낼 수 있는 Routing Policy
   - 요청이 발생한 지점에서 가장 가까운 위치의 리소스로 라우팅
   - 💡 즉, 요청한 지역과 리소스의 거리 기반

2. 추가적으로 Bias를 지정해 지역 범위 조절 가능 (Bias : 특정 지역이 더 많은 범위 혹은 더 적은 범위를 커버하도록 조절하는 보정값)
3. 주요 사용 사례
   - 지역별 컨텐츠 제공 구분
   - 최소 지연 속도로 라우탕
<div align="center">
<img src="https://github.com/user-attachments/assets/1009b2fc-96b5-47a8-9d73-c15eedb88ea9" />
</div>

-----
### Latency-Based Routing Policy
-----
1. 유저 기준으로 가장 빠른 Latency(네트워크 지연 시간)을 가진 레코드를 라우팅 하는 정책
   - 예) 도쿄 리전 / 버지니아 리전에 ALB가 있을 때, 서울 리전에서 요청한 경우, 서울 → 도쿄 / 서울 → 버지니아를 비교하여 가장 빠른 리전으로 라우팅

2. 주의 : AWS 데이터 센터 간의 Latency를 기준으로 하므로 AWS 외부의 소스일 경우 정확도가 매우 떨어짐
3. 주요 사용 사례
   - 여러 리전 간 최적화된 유저 경험을 위한 라우팅
   - Active-Active Failover
<div align="center">
<img src="https://github.com/user-attachments/assets/2627dd6d-684b-4836-b885-d5bb1d8cc44a" />
<img src="https://github.com/user-attachments/assets/26352228-565e-45a4-affc-9aff5e09f229" />
</div>

-----
### IP-Based Routing Policy
-----
1. IP 기반으로 라우팅을 조절하는 정책 : 기존의 Geolocation / Latency-Based 라우팅 등에 추가로 네트워크 이해를 바탕으로 정교한 라우팅 정책 구성 가능
2. CIDR Block Range 별로 다른 라우팅 구성 가능
3. 주요 사용 사례
   - 특정 네트워크를 구분하여 라우팅 : 예) 특정 ISP 대역만 분리하여 라우팅 하고 싶은 경우 / 회사 IP만 Dev 리전으로 라우팅
<div align="center">
<img src="https://github.com/user-attachments/assets/530b04f7-6eaf-449b-8d30-923c7582bcef" />
</div>

-----
### Multivalue Routing Policy
-----
1. 한 번의 요청으로 다양한 값을 전달하는 정책 : Health Check와 연동하여 현재 원활한 상태의 값만 선별적 보내기 가능
2. 주요 사용 사례 : 로드 밸런싱 / 간단한 Failover
<div align="center">
<img src="https://github.com/user-attachments/assets/531431c1-6a5a-41bd-81bf-b295b6068812" />
</div>

-----
### Weighted Routing Policy
-----
1. 다수의 리소스를 하나의 도메인으로 묶어 각 리소스에 비중을 두고 분배하는 정책
   - 같은 이름과 타입의 레코드를 만들어 비중(Weight)을 부여

2. 비중 (Weight) : 1 ~ 255의 값으로 높은 비중을 가진 레코드일수록 더 많은 트래픽 분배
3. 주요 사용 사례
   - 로드밸런싱
   - A / B 테스트
   - Canary 릴리즈
   - Active-Active Failover
<div align="center">
<img src="https://github.com/user-attachments/assets/7257e6fa-e9e4-4602-9665-0788bf0093e9" />
</div>

