-----
### Backtrack
-----
1. 기존의 DB를 특정 시점으로 되돌리는 것(새로운 DB가 아닌 기존 DB)
   - DB 관리 실수를 쉽게 만회 가능 (예) Where 없는 Delete)
   - 새로운 DB를 생성하는 것보다 훨씬 빠름
   - 앞 / 뒤로 시점을 이동 할 수 있으므로 원하는 지점을 빠르게 찾을 수 있음

2. Backtrack Window
   - Target Backtrack Window : 어느 시점 만큼 DB를 되돌리기 위한 데이터를 저장할 것인지 지정 (지정 시점 이전으로는 Backtrack 불가)
   - Actual Backtrack Window : 실제로 시간을 얼만큼 되돌릴지 지정 (Target Backtrack Window보다 작아야 함)

3. Backtrack 활성화 시, 시간당 DB 변화를 저장
   - 저장된 용량만큼 비용 지불
   - DB 변화가 많을수록 많은 로그 = 많은 비용
   - DB 로그가 너무 많아 Actual Backtrack Window가 Target Backtrack Window(설정값)보다 작을 경우, 알림

4. MySQL만 가능 : Aurora 생성 시, Backtrack을 설정한 DB만 가능
5. 스냅샷을 복구하거나 Clone을 통해 활성화 가능
6. 💡 다운타임 존재
<div align="center">
<img src="https://github.com/user-attachments/assets/0618bbb5-df97-4e9c-a1c9-f757af71e2af" />
</div>

-----
### Demo
-----
1. Aurora을 프로비전하고 Backtrack 확인 : 1분 단위 레코드를 입력하고 원하는 시간으로 Backtrack
2. MySQL 워크벤치 사용
3. 비용 발생 가능
4. RDS 생성
   - 데이터베이스 생성 : Auroa (MySQL Compatible)
   - 개발 / 테스트용
   - my-aurora
   - 자격 증명 관리 : 자체 관리 후, 마스터 암호 입력
   - 인스턴스 구성 - 버스터블 클래스 - db.t3.medium
   - 퍼블릭 액세스 활성화
   - 추가 구성 : 초기 데이터베이스 이름은 my_db / 역추적 - 역추적 활성 (대상 역추적 시간 : 2)

5. my-aurora (Cluster) - 연결 및 보안 - 엔드포인트 2개 (Region Cluster, Writer Instance)
   - 현재 Writer Instance가 Read / Write 모두 실시하지만, 본래 분리
   - Read Instance가 여러 개일 경우, 해당 Read Instance 대표로 접근해주면 자동으로 연결
   - MySQL 워크벤치 연결
     + Write Instance 엔드포인트 사용 : 기존 my-db-test-connection에서 Hostname만 해당 엔드포인트로 변경 (TCP/IP)
     + 테이블 생성 : tb_test (idx : auto_increment / value : VARCHAR(45) / created_datetime : DATETIME (기본값 : now())
     + 1분 마다 데이터 생성 (5개 입력)
   - Backtrack
     + my-aurora 선택 후 작업 - 역추적 - 원하는 시점 선택 후 역추적 실시
     + MySQL에서 데이터 확인

  6. 리소스 정리 : my-aurora-instance 삭제 후, Cluster 삭제
