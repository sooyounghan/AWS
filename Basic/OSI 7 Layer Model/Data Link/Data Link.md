-----
### Data Link Layer
-----
1. 물리적인 통신을 제어하며 디바이스와 디바이스 간 통신 및 전송을 안정화하기 위한 프로토콜을 지원
2. 주요 단위 : Frame
3. 주요 구성 요소 : Mac Address, Switch
4. 주요 특징
   - CSMA / CD(Carrier-Sense Multiple Access with Collsion Detection) 방식을 활용해서 각 디바이스 간 통신을 원할하게 연
   - 대상을 구별하여 디바이스 간 통신을 지원 (Unicast) / Broadcast도 지원

5. MAC(Media Access Control) Address
   - 네트워크 인터페이스에 부여된 고유의 주소 : 데이터가 지정한 대상에게 잘 전달될 수 있도록 대상 식별에 사용
   - 2개의 Hexadecimal(= Byte) 단위로 6개를 나열 = 48 Bits = 6 Bytes (예) ```00:1A:2B:3C:4D:5E```)
   - 두 파트로 구분
     + 첫 3개 Bytes는 OUI (Organizationally Unique Indetifier) : 제조사에 부여된 고유 식별자
     + 나머지 3개 Bytes는 NIC (Network Interface Controller) : 네트워크 인터페이스 별 고유 번호

   - 네트워크 인터페이스의 MAC Address는 고유 값이며 변하지 않음 (단, IP는 변동)

6. Switch (L2)
<div align="center">
<img src="https://github.com/user-attachments/assets/28440727-134e-480b-903a-ea7f35506a3a">
</div>

   - L2 디바이스이며, 각 네트워크 인터페이스에 MAC 주소가 부여되어 있는 상태
   - 따라서, A가 B한테 직접 전송 가능

7. Frame
<div align="center">
<img src="https://github.com/user-attachments/assets/0ae9d58b-2bc9-4846-a823-6bbc933d1a1f">
</div>

   - Layer2에서 사용되는 일종의 통신 단위
   - Preamble : Frame의 시작을 알림 
   - 대상 MAC Address : 보내는 대상을 의미, 소스 MAC Address : 데이터를 받는 대상
   - Ehtertype : Frame의 길이 및 Layer 3 프로토콜을 명시
   - Payload : 실제 데이터 내용
   - Frame Check Sequence : 오류 검증

<div align="center">
<img src="https://github.com/user-attachments/assets/18d1fc60-0f67-4500-80cd-442a12ab382a">
</div>

8. 연결 흐름도 : 데이터 링크 레이어 하단에 물리 레이어가 존재
   - 물리 레이어에서 물리적 통신을 실시하는데, Frame을 비트 스트림으로 생성해 전달
<div align="center">
<img src="https://github.com/user-attachments/assets/513d5004-0c4d-409a-bbed-81d64bca7968">
</div>

9. CSMA / CD(Carrier-Sense Multiple Access with Collsion Detection)
    - Carrier-Sense : 물리 레이어에 신호가 전송되는데 이를 확인하는데, 아무것도 없다면 Frame을 전송
<div align="center">
<img src="https://github.com/user-attachments/assets/1c1d4056-b1ac-4779-93f9-eab06a2b584a">
</div>

   - Multiple Access : 여러 개를 한 번에 사용한다는 의미
   - 만약, 이 두 전송이 서로 없다고 판단하여 메세지가 충돌하게 되면, 확인하는 것이 Collsion Detection
      + 두 디바이스 모두 멈추고, 시그널을 보내 충돌 발생을 서로가 인지를 할 수 있도록 함
      + 두 디바이스 모두 랜덤한 시간을 기다리며, 해당 시간이 지나면 전송
      + 하지만, 또 다시 충돌이 발생하면 조금 더 오래 기다려서 충돌을 해결

10. Data Link Layer 계층에도 충돌 방지 기능이 있다고 할지라도, 주체들이 많으면 비효율적
    - 따라서, 이를 스위치 개념으로 묶어서 관리하면, 충돌을 굉장히 줄일 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/6430bdc0-2d0a-4bbe-9de6-71757bd3e8c2">
</div>

   - 스위치 안에 테이블이 존재하며, 스위치 포트 별로 어떤 디바이스가 있는지 기록하고 있음
   - 프레임을 저장하는 공간이 존재 (Frame Storage)
   - 클라이언트 A가 클라이언트 D에게 통신을 하고 싶다면, A에서 D로 전송된느 구간만 생각하면 됨 : 스위치는 각 구간을 따로 관리하므로 B, C, D를 생각하지 않아도 됨
   - 충돌이 발생한다 할지라도, CSMA / CD를 이해하고 있으므로, 조금 기다렸다가 다시 보내도록 설정

11. Broadcast
<div align="center">
<img src="https://github.com/user-attachments/assets/7704fd78-1375-44e2-bdd6-c0c8f02e29e6">
</div>

   - 대상 MAC Address를 ```FF:FF:FF:FF:FF:FF```로 지정하여 Frame 생성
   - 스위치를 통해 전송

12. 문제점 : 로컬 외부 네트워크로의 통신 불가
