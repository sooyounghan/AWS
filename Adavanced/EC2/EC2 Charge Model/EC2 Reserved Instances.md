-----
### 예약 인스턴스
-----
1. EC2 인스턴스를 일정 기간 약정하여 요금을 할인 받는 방식
   - On-Demand EC2 사용 요금을 할인 받는 방식으로 적용
   - 할인 받고 싶은 EC2 인스턴스와 같은 리전, 유형 구매 필요

2. 약정 기간이 길수록 더 큰 할인율 적용 : 1년 혹은 3년 선택 가능
3. Region 별로 적용 : 다른 Region과 공유 불가
4. 구성 요소
   - 인스턴스 유형 : m4.large (유형 : m4 / 크기 : large)
   - 테넌시 : 전용 호스트 혹은 공유 하드웨어
   - 플랫폼 : Windows 또는 Linux
   - 기간 약정 : 1년 또는 3년
   - 결제 방법
     + 전체 선결제 : 모든 금액을 기간 시작 전 결제 (가장 저렴)
     + 부분 선결제 : 비용 중 일부만 시작 전 결제 (나머지 비용은 할인 가격으로 시간 당 청구)
     + 선결제 없음 : 모든 비용을 할인 가격으로 시간당 청구

   - 제공 클래스
     + 표준 : 큰 할인 혜택 (단, 교환 불가능 / 수정만 가능)
     + 컨버터블 : 낮은 할인 혜택 (다른 속성의 예약 인스턴스로 교환 가능)

5. 범위에 따른 종류
   - 리전 : 리전 전체에 사용할 수 있는 예약 인스턴스
     + 미리 용량 예약 불가능
     + 인스턴스 크기 유연성 적용 : 크기에 상관없이 인스턴스 패밀리의 사용량으로 예약 인스턴스 소모
     + 구매 대기 가능

   - 영역 : 특정 가용영역에서만 사용할 수 있는 예약 인스턴스
     + 미리 용량 예약 가능 = 따로 해당 유형의 인스턴스를 확보
     + 구매 대기 불가능

6. 제공 클래스에 따른 종류
   - 표준 (Standard)
     + 교환 불가능
     + 예약 인스턴스 마켓 플레이스에 판매 / 구매 가능

   - 전환형 (Convertible)
     + 인스턴스 유형, 플랫폼, 범위 또는 테넌스 등의 다른 속성의 전환형 예약 인스턴스와 교환 가능
     + 예약 인스턴스 마켓플레이스에 판매 / 구매 불가능

7. EC2 예약 인스턴스 적용 방식
<div align="center">
<img src="https://github.com/user-attachments/assets/ae1791fc-71f5-4b8f-99cf-0d013b0e0084" />
</div>

   - 구매 즉시 할인 가격 적용
   - 정확하게 설정한 속성과 맞아야 함 (인스턴스 유형 / 플랫폼 / 가용영역 / 인스턴스 사이즈 등)
   - 💡 인스턴스 사이즈 유연성 (리전 예약 인스턴스 Only)
     + 각 인스턴스 사이즈별로 사용 점수 보유
     + 구매한 RI의 점수로 사용중인 인스턴스 점수를 커버, 남는 부분은 On-Demand로 과금
     + 지원하지 않는 OS / 인스턴스 타입 존재
       * 지원하지 않는 OS 예) Windows Server (SQL 서버) / RHEL, SUSE Linux Enterprise Server
       * 지원하지 않는 인스턴스 타입 예) G4ad, G4dn, G5, G5g, Inf1, Inf2 인스턴스 타입

8. 유연성 (Size-Flexible) (리전 예약 인스턴스 Only)
<div align="center">
<img src="https://github.com/user-attachments/assets/bda2630b-fe12-49ab-bf89-603130d435f5" />
</div>

   - Size-Fiexible Unit
<div align="center">
<img src="https://github.com/user-attachments/assets/f0f041e7-0cbc-487d-b45d-5af5785e44d5" />
<img src="https://github.com/user-attachments/assets/d8862d5b-9517-4c13-98a2-3d23277e6774" />
<img src="https://github.com/user-attachments/assets/ef52446c-b949-4744-962e-eba9ea6cd0a8" />
<img src="https://github.com/user-attachments/assets/647692ab-de0e-44c0-827b-843f5dc872e7" />
</div>

9. 인스턴스 사이즈 유연성 적용 방식
    - t2.medium을 리전 RI로 구매할 경우
      + 확보 점수 : 2점
      + 2개의 t2.small(1점) 커버 가능
      + 4개의 t2.micro(0.5점) 커버 가능
      + 절반의 t2.large(4점) 커버 가능

   - 작은 인스턴스에서 큰 인스턴스로 커버
   - 특정 OS / 인스턴스 타입의 경우 적용 불가능

10. 예약 인스턴스 적용
    - 매시 정각에 비용 발생 (선결제 없을 경우)
      + 오픈 소스 리눅스(Amazon Linux 등)의 경우 할인 혜택 자체는 초단위로 적용
      + 상용 리눅스(Red Linux 등)의 경우 할인 혜택이 시간 단위 적용

    - 한 RI 당 한 시간의 인스턴스만 커버
      + m4.large RI 하나만 구매 후 m4.xlarge 4개를 1시간 동안 구동했을 경우 : 1개만 커버되고, 나머지 3개는 On-Demand 요금 적용
      + m4.xlarge RI 하나 구매 후, m4.large 4개를 15분씩 구동했을 경우 : 총 합이 60분 = 1시간이므로 모두 RI로 커버

11. EC2 예약 인스턴스 비용 청구
<div align="center">
<img src="https://github.com/user-attachments/assets/f01479b9-7b73-42d1-8f1c-a48a3e17c33c" />
<img src="https://github.com/user-attachments/assets/e81c5bb6-685a-4ebf-bd84-c6f5f95f4270" />
<img src="https://github.com/user-attachments/assets/5febe3bd-cae5-4964-a086-8a5f87087569" />
</div>

12. 전환형 (Convertible) 예약 인스턴스의 교환
    - 전환형 RI의 경우 교환 가능
      + 규칙에 맞는 교환이라면 횟수 제한 없음
      + 조건 : 내가 보유한 RI 가치 ≤ 받으려는 RI 가치

    - 규칙
      + 현재 AWS에서 제공하는 RI 타입으로만 교환 가능
      + 다른 리전이랑 교환 불가능
      + 예약 인스턴스 한개씩만 교환 가능 (받을 때, 여러 인스턴스로 받을 가능성)
      + 일부만 교환하고 싶을 때는, 예약 인스턴스를 작은 단위로 나눈 후 각 단위를 교환
      + 약정 기간이 같은 RI만 교환 가능 (1년, 3년)
<div align="center">
<img src="https://github.com/user-attachments/assets/eb87986c-66b8-4584-ad4b-e7b98e6584a6" />
</div>

   - 수정
     + 가용영역, 인스턴스 사이즈(같은 유형), 범위 변경 가능
     + 분할 가능
       * 1 X t2.small = 2 x t2.micro
       * 10개 AZ a RI = 5개 AZ a, 5개 AZ b
     + 병합 가능 : 2 x t2.micro = 1 x t2.small
     + 인스턴스 사이즈 변경은 리눅스 / 유닉스만 가능
     + 몇 가지 인스턴스 유형은 불가능(G4)
<div align="center">
<img src="https://github.com/user-attachments/assets/a773c07f-25eb-4643-84c1-a4f545504050" />
</div>

13. 예약 인스턴스 구매 / 판매
    - AWS 마켓 플레이스에서 다른 사람이 판매하는 RI 구매 가능 : 구매한 RI가 필요 없어진 경우 판매 가능
    - 표준(Standard) RI만 판매 / 구매 가능
    - 판매 선결제 금액의 12%를 AWS에서 수수료로 가져감
    - 미국은행에 계좌가 있어야 판매 가능
    - 💡 경우에 따라 MSP에 판매 가능
  
  
