-----
### 가상화
-----
1. 가상화 : 단일 컴퓨터 하드웨어 요소를 일반적으로 가상 머신(VM)이라고 하는 다수의 가상 컴퓨터로 분항할 수 있도록 해주는 기술
<div align="center">
<img src="https://github.com/user-attachments/assets/9b8a2934-54eb-4def-a384-caa447871762" />
</div>

2. 운영체제 (Operating System, OS) : 시스템 하드웨어 자원과 소프트웨어 자원을 운영 관리하는 프로그램 (Windows, Linux, MacOS, Android 등)
3. 특권 명령(Privileged Instruction) : 시스템 요소들과 소통할 수 있는 명령으로, OS(Kernel)만 가능
   - OS는 특권 명령 때문에 하나의 하드웨어 시스템 당 하나밖에 돌아갈 수 밖에 없음
   - 일반 프로그램은 특권 명령이 필요 없으므로 많은 프로그램을 동시에 수행 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/b598dbbf-cb7c-4c09-9bc4-3a4168e2a09f" />
</div>

4. 가상화가 나타나기 전까지는 하나의 하드웨어 시스템은 하나의 OS만 실행 가능
   - 즉, 일반적 컴퓨터처럼 직접 OS가 하드웨어에 설치된 상태(Bare-Metal)로만 운영 가능

5. 가상화의 역사
   - 1세대 : 완전 가상화(Fully Emulated)
     + 모든 시스템 요소가 Emulator 안에서 동작 : 즉, CPU / 하드디스크 / 마더보드 등 모든 요소를 애뮬레이터로 구현하여 Guest OS(가상화 환경 OS)와 연동
     + 엄청나게 느림
<div align="center">
<img src="https://github.com/user-attachments/assets/0607c581-a352-44e9-9526-c04093ba85ef" />
</div>

   - 2세대 : Paravirtualization
     + Guest OS는 Hypervisor와 통신 (하이퍼바이저(Hypervisor : OS와 하드웨어 사이 존재하는 일종의 가상화 매니저)
     + 속도의 향상
     + 몇몇 요소의 경우 여전히 Emulator 필요하므로 느림
<div align="center">
<img src="https://github.com/user-attachments/assets/47d4c19c-c55c-4c5c-b96f-b2b447a72b43" />
</div>

   - 3세대 : Hardware Virtual Machine (HVM)
     + 하드웨어에서 직접 가상화 지원
     + 직접 Guest OS가 하드웨어와 통신하므로 빠른 속도(Near Bare-Metal)
<div align="center">
<img src="https://github.com/user-attachments/assets/871b6afa-942f-4612-9799-fad9e152f948" />
<img src="https://github.com/user-attachments/assets/0b69ec6a-69fc-45f0-884b-d31b28ae6df3" />
</div>

6. 가상화와 클라우드
   - AWS 클라우드 환경에서 리소스를 작은 단위로 빠르게 구성할 수 있는 원동력은 가상화
   - 즉, AWS에서 사용자마다 컴퓨터를 할당해주는 것이 아니라 이미 구축된 가상화 가능한 서버의 한 부분을 할당해주는 것
<div align="center">
<img src="https://github.com/user-attachments/assets/2a0a23e2-cc5b-478a-af48-b847fde4aefe" />
</div>
