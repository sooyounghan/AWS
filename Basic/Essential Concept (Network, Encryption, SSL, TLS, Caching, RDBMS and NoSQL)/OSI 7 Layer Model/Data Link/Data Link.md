-----
### Data Link Layer
-----
1. 물리적인 통신을 제어하며 디바이스와 디바이스 간 통신 및 전송을 안정화하기 위한 프로토콜을 지원
2. 주요 단위 : Frame
3. 주요 구성 요소 : Mac Address, Switch
4. 주요 특징
   - CSMA / CD(Carrier-Sense Multiple Access with Collsion Detection) 방식을 활용해서 각 디바이스 간 통신을 원할하게 연결
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
<img src="https://github.com/user-attachments/assets/6d56dcc6-08c7-47fe-82ba-88d89110a18b" />
</div>

   - L2 디바이스이며, 각 네트워크 인터페이스에 MAC 주소가 부여되어 있는 상태
   - 따라서, A가 B한테 직접 전송 가능

7. Frame
<div align="center">
<img src="https://github.com/user-attachments/assets/cbcdeffa-0c76-4be2-9046-6a3b0e5880d2" />
</div>

   - Layer2에서 사용되는 일종의 통신 단위
   - Preamble : Frame의 시작을 알림 
   - 대상 MAC Address : 보내는 대상을 의미, 소스 MAC Address : 데이터를 받는 대상
   - Ehtertype : Frame의 길이 및 Layer 3 프로토콜을 명시
   - Payload : 실제 데이터 내용
   - Frame Check Sequence : 오류 검증

<div align="center">
<img src="https://github.com/user-attachments/assets/0a72de7f-f7f8-4aab-9341-88e0fcd4911f" />
</div>

8. 연결 흐름도 : 데이터 링크 레이어 하단에 물리 레이어가 존재
   - 물리 레이어에서 물리적 통신을 실시하는데, Frame을 비트 스트림으로 생성해 전달
<div align="center">
<img src="https://github.com/user-attachments/assets/41c3efab-13c1-4f4b-98ac-31919247813d" />
</div>

9. CSMA / CD(Carrier-Sense Multiple Access with Collsion Detection)
    - Carrier-Sense : 물리 레이어에 신호가 전송되는데 이를 확인하는데, 아무것도 없다면 Frame을 전송
<div align="center">
<img src="https://github.com/user-attachments/assets/1e602b05-50d3-47db-8f5e-cacd4a0877b3" />
<img src="https://github.com/user-attachments/assets/06002989-c0a7-412e-8248-0e6ed2c92fcc" />
<img src="https://github.com/user-attachments/assets/db559cd6-f382-44d8-ab9e-2407159150d7" />
<img src="https://github.com/user-attachments/assets/1b2510b8-da58-4938-b7c5-4b725da12d6a" />
<img src="https://github.com/user-attachments/assets/e4c08f9f-4f73-4da7-aa52-8f45a1433f2f" />
<img src="https://github.com/user-attachments/assets/8b89faa8-24bb-4cd8-84f1-2bab10d1158c" />
<img src="https://github.com/user-attachments/assets/fdd86a52-4c08-4f24-9652-b84a5a40d09e" />
<img src="https://github.com/user-attachments/assets/9d1f82fe-c377-4524-840e-ff50eede0927" />
</div>
   - Multiple Access : 여러 개를 한 번에 사용한다는 의미
   - 만약, 이 두 전송이 서로 없다고 판단하여 메세지가 충돌하게 되면, 확인하는 것이 Collsion Detection
      + 두 디바이스 모두 멈추고, 시그널을 보내 충돌 발생을 서로가 인지를 할 수 있도록 함
      + 두 디바이스 모두 랜덤한 시간을 기다리며, 해당 시간이 지나면 전송
      + 하지만, 또 다시 충돌이 발생하면 조금 더 오래 기다려서 충돌을 해결

10. Data Link Layer 계층에도 충돌 방지 기능이 있다고 할지라도, 주체들이 많으면 비효율적
    - 따라서, 이를 스위치 개념으로 묶어서 관리하면, 충돌을 굉장히 줄일 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/b588db7b-1eae-4cb3-982e-b981ce24e649" />
<img src="https://github.com/user-attachments/assets/343bdda4-985d-4263-87c1-85da338da250" />
<img src="https://github.com/user-attachments/assets/43603f3e-218f-427a-88d3-2b5450dd6616" />
<img src="https://github.com/user-attachments/assets/809e5355-7cbc-491d-b748-006520e8c1f7" />
</div>


   - 스위치 안에 테이블이 존재하며, 스위치 포트 별로 어떤 디바이스가 있는지 기록하고 있음
   - 프레임을 저장하는 공간이 존재 (Frame Storage)
   - 클라이언트 A가 클라이언트 D에게 통신을 하고 싶다면, A에서 D로 전송된느 구간만 생각하면 됨 : 스위치는 각 구간을 따로 관리하므로 B, C, D를 생각하지 않아도 됨
   - 충돌이 발생한다 할지라도, CSMA / CD를 이해하고 있으므로, 조금 기다렸다가 다시 보내도록 설정

11. Broadcast
<div align="center">
<img src="https://github.com/user-attachments/assets/2e24e558-8f39-4f3c-bcc8-f53029d5af80" /
</div>

   - 대상 MAC Address를 ```FF:FF:FF:FF:FF:FF```로 지정하여 Frame 생성
   - 스위치를 통해 전송

12. 문제점 : 로컬 외부 네트워크로의 통신 불가
