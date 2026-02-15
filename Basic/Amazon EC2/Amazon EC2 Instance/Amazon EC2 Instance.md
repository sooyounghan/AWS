-----
### EC2 Instance
-----
1. EC2 구조
<div align="center">
<img src="https://github.com/user-attachments/assets/5cea8b63-6f6b-460f-af61-22881d334705" />
</div>

2. EC2 인스턴스
   - EC2에서 컴퓨팅을 담당
     + 다양한 유형과 크기로 구성
     + 저장을 담당하는 EBS와 네트워크로 연결

   - 저장 방법에 따라 두 가지로 분류
     + EBS 연동 : 하드디스크에 해당하는 EBS와 네트워크 연결
     + 인스턴스 스토어 : EC2 안에 스토리지 존재

   - 💡 하나의 가용영역(AZ)에 존재 (즉, 여러 가용 영역에서 걸쳐서 존재 불가)
<div align="center">
<img src="https://github.com/user-attachments/assets/ae5a3b3e-7f8c-47a5-9fde-2baf054e3a29" />
<img src="https://github.com/user-attachments/assets/0d84f282-6087-4173-a268-321df1da16fe" />
</div>

3. 인스턴스 유형
   - 인스턴스 유형 (패밀리)
     + 인스턴스 역할에 따라 CPU, 메모리, 스토리지, 네트워크 등을 조합한 구성
     + 각 인스턴스 유형 별 사용 목적에 따라 최적화 (예) 메모리 위주, CPU 위주, 그래픽 카드 위주 등)
     + 유형 별로 이름 존재 : 같은 유형의 인스턴스들을 인스턴스 패밀리라고 부름 (예) t유형, m유형, inf유형 등)
     + 타입 별 세대별로 숫자 부여 (예) m5 = m인스턴스의 5번째 세대)
     + 아키텍쳐 및 프로세서 / 추가기술에 따라 접미사 (2개 이상 가능) (c7gn = c 인스턴스 중 AWS Graviton 프로세서를 사용(g) + Network Optimized(n) = c7gn)
<div align="center">
<img src="https://github.com/user-attachments/assets/04a4009b-5cd5-494b-a835-98429d041c71" />
</div>

4. 인스턴스 크기
   - 같은 인스턴스 패밀리에 다양한 크기 존재
   - 인스턴스 CPU 개수, 메모리 크기, 성능 등으로 크기 결정
   - 크기가 클수록 더 많은 메모리, 더 많은 CPU, 더 많은 네트워크 대역폭, EBS와의 통신 가능한 대역폭
<div align="center">
<img src="https://github.com/user-attachments/assets/7f458f61-22a2-4550-bf32-911fa8815162" />
</div>

5. 인스턴스 유형 읽는 방법
<div align="center">
<img src="https://github.com/user-attachments/assets/a597e7c5-7242-47d2-9273-f575960048d2" />
</div>

