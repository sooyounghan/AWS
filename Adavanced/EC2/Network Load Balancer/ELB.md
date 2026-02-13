-----
### ELB의 종류
-----
1. Application Load Balancer
   - OSI Model Layer 7
   - 트래픽을 모니터링하여 라우팅 가능
   - 예) ```image.sample.com``` : 이미지 서버로 트래픽 분산
   - 예) ```web.sample.com``` : 웹 서버로 트래픽 분산 

2. Network Load Balancer
   - OSI Model Layer 4
   - TCP, UDP 기반 빠른 트래픽 분산
   - Elastic IP 할당 가능 (IP 고정 가능)

3. Classic Load Balancer : 예전에는 사용되던 타입으로 현재는 잘 사용하지 않음

4. Gateway Load Balancer
   - OSI Model Layer 3
   - 가상 Appliance 배포 / 확장 관리를 위한 서비스

  
