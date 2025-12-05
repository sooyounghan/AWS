-----
### Physical Layer
-----
1. OSI 7 Layer 중 가장 맨 밑단에 위치하고 가장 기본적인 레이어
2. 장치를 연결하기 위한 매체의 물리적 사항 정의 (전압, 주기, 시간, 전선 규격, 거리 등)
3. 주요 단위 : Bits
4. 대표 구성 요소 : 케이블 / 안테나 / RF 등 전송 매체, 허브, 리피터
5. 두 주체 간에 데이터를 교환하는 방식
<div align="center">
<img src="https://github.com/user-attachments/assets/84e501af-9298-4aa6-9e6c-61635db00f0f">
</div>

   - 전달 방식
     + 비트라고 불리는 데이터에 스트림을 전송하는데 1과 0으로 구성
     + 예를 들어, 구리선이라고 한다면, Voltage가 +면 1, -이면 0으로 정의
     + 예를 들어, 전파라고 한다면 1이면 전송, 0이면 없음의 형식으로 정의
<div align="center">
<img src="https://github.com/user-attachments/assets/9af73f90-b1fd-4581-a439-f0cced21bccb">
</div>

6. 물리적 연결 수립의 예
<div align="center">
<img src="https://github.com/user-attachments/assets/d637db40-1157-42be-85e2-9dba8cf370db">
</div>

   - Hub : Physical Layer 단위에서 다수의 기기들을 연결해주는 장치
   - 특징과 그에 따른 문제점
     + 에러 / 충돌(Collision) / 디바이스 별 제어 기능이 없음
     + 받은 내용을 그대로 전달 (Broadcast) : 즉, 대상을 지정해서 전달 불가능
     + 물리적 Layer에 허브(L1)를 연결한 그림 : 허브가 하는 역할은 받은 신호를 그대로 다른 클라이언트에게 전달
<div align="center">
<img src="https://github.com/user-attachments/assets/99d14dbc-78cf-4ef0-940e-b1dd2d4433d0">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/dff85fc3-2673-48dd-8727-311dd84da6ca">
</div>

   - 문제점 : 물리적으로 선으로 연결되어있는데, 만약 클라이언트 A, B, C, D가 동시에 전송하면 누구도 받지 못하는 충돌(Collision) 문제가 발생 : 하지만, 허브(Hub)에는 충돌을 막거나 조절할 수 있는 기능이 아예 없음
      + 또한, Client A가 Client B로 직접 전송하고, D라든지 C는 듣지 못하게 대상을 지정해서 할 수 없음 : 허브는 데이터를 받으면 전송하는 역할만 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/3a3bcde1-fe2b-436c-b077-80c631fc6740">
</div>

   - 이러한 기능을 해결하기 위해서는 Data Link에서 해결해야 함
   
