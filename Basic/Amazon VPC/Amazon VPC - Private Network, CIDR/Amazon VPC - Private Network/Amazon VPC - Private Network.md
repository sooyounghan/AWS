-----
### 사설망 (Private Network)
-----
1. 한정된 IP 주소를 최대한 활용하기 위해 IP 주소를 분할하고자 만든 개념 : IPv4 기준 최대 IP 갯수는 43억개 (4,294,967,296)
2. 사설망
   - 사설망 내부에는 외부 인터넷 망으로 통신이 불가능한 사설 IP로 구성
   - 외부로 통신할 때는 통신 가능한 공인 IP로 나누어 사용
   - 보통 하나의 망에는 사설 IP를 부여받은 기기들과 NAT 기능을 갖춘 Gateway로 구성
   - Private IP는 지정된 대역의 아이피만 사용 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/af834575-1037-4357-8091-3a55f7d601f0" />
</div>

3. 참고 : IPv6 최대 개수 ($2^{128}$ = 340282366920938463463374607431768211456개)
   - IPv4보다 79228162514264337593543950336(= $2^{96}$)배 더 많음
   - 따라서 사설망이 필요 없음

-----
### NAT (Network Address Tralsation)
-----
1. 사설 IP가 공용 IP로 통신할 수 있도록 주소를 변환해주는 방법
2. 3가지 종류
   - Dynamic NAT : 1개의 사설 IP를 가용 가능한 공인 IP로 연결 (공인 IP 그룹(NAT Pool)에서 현재 사용 가능한 IP로 가져와서 연결)
<div align="center">
<img src="https://github.com/user-attachments/assets/e563c5c2-4dd4-4798-9840-839844871bab" />
<img src="https://github.com/user-attachments/assets/6d598611-26ca-4713-8706-b2e8c994ed75" />
</div>

   - Static NAT : 하나의 사설 IP를 고정된 하나의 공인 IP로 연결 (AWS Internet Gateway가 사용하는 방식)
<div align="center">
<img src="https://github.com/user-attachments/assets/60ce28b9-c85a-4838-999e-ad90ae30a270" />
<img src="https://github.com/user-attachments/assets/ae70e947-e204-4f36-99e9-86d177604f00" />
</div>

   - PAT(Port Address Translation) : 많은 사설 IP를 하나의 공인 IP로 연결 (NAT Gateway / NAT Instance가 사용하는 방식)
<div align="center">
<img src="https://github.com/user-attachments/assets/97121579-dcec-4bce-8223-42b84cc1b987" />
<img src="https://github.com/user-attachments/assets/d0aa5801-3b4d-4b41-b03a-05a22e7fcde6" />
<img src="https://github.com/user-attachments/assets/5c3026b1-ece6-44f1-be82-9694e3bcbd61" />
</div>

