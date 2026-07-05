## AWS-Cloud-Openvpn 기초 실습 
--------------------------------------------------------------------------------
# AWS Cloud OpenVPN 클라이언트 설치, ping test, 최초 ping test 실패 원인 분석.

# 1. EC2에서 OpenVPN Access Server Community Image 생성.
- 퍼블릭 IP 활성화 선택.
- 보안 그룹 생성(구분하기 쉽게 이름 설정).

  <img width="1377" height="863" alt="Image" src="https://github.com/user-attachments/assets/7b64797c-2d3f-4513-b496-e43be45f9562" />

----------------------------------------------------------------------------------

# 2. EC2 연결하여 OpenVPN 설치.

- EC2 SSH 연결 후, 자동으로 OpenVPN 설치 시작.
- OpenVPN 라이센스 계약서 내용이 차례대로 나오면 "YES" 입력하여 동의하고 진행.
- 도중에 Primary(본서버)로 만들지 백업서버로 만들지 선택지에서 "yes"입력하여 Primary 로 선택(ping test용으로 하나만 사용할 것임).
  - 만약 Primary의 장애발생을 대비한 Backup 용 서버를 설치하는 경우에는 "no"를 입력(고가용성을 위하여).
- "Please specify your Activation key (or leave blank to specify later" 라고 문구가 뜸. 키 없는 상태로 진행하기 위해 enter 입력.
  - 테스용 or 2명 이하만 이용 예정이면 "enter".
  - 유료 라이센스 키를 이미 구매했다면, 구매 시 받은 키를 복사 붙혀넣기 하고 "enter".
  - 키를 아직 구매하지 않았고, 나중에 구매할 예정이라면 "enter" 치고, 설치 완료 후 관리자 페이지에서 키 활성화 가능.
- 설치 완료 후, CLI에서 OpenVPN 관리자 페이지의 URL과 로그인을 위한 계정, Password를 확인 할 수 있음. 잘 보관 할것.
  
<img width="362" height="32" alt="Image" src="https://github.com/user-attachments/assets/75d91d37-9bde-4924-a1e7-24b5109058f4" />

-------------------------------------------------------------------------------------------------------------------------------

# 3. OpenVPN 웹사이트 로그인 및 클라이언트 다운.

- CLI에서 확인 한 URL에 브라우저를 통해 접속.
- CLI에서 확인 한 계정과 Password로 로그인(Password는 관리자 페이지에서 변경도 가능).
- AI에 OpenVPN 클라이언트 URL 요청하여 다운.
- 메인화면에서 USER 카테고리 클릭, 화면 중앙 "Openvpn" 이라고 쓰여있는 곳에서 맨 우측에 점3개 아이콘 우클릭하여" Profile download" 클릭.
- OpenVPN 클라이언트 실행하여 관리자 페이지와 동일하게 로그인. Profile Switch 란에 다운받은 Profile 업로드 후 "Connect" 클릭.
- cmd창 열고, __"ping [OpenVPN Server의 프라이빗 IP]"__ 입력하여 ping test 실행. 손실률 0% 나와야 접속 테스트 성공.

<img width="1423" height="262" alt="Image" src="https://github.com/user-attachments/assets/d43f9a21-f529-4f2a-a9a2-62452e60b793" />


<img width="687" height="677" alt="Image" src="https://github.com/user-attachments/assets/507de1cc-b130-4df7-b779-4fb5c55160da" />


<img width="472" height="487" alt="Image" src="https://github.com/user-attachments/assets/8a2382aa-be80-4b79-b496-5bd341db9dc1" />

------------------------------------------------------------------------------------------------------------------------------------

## 4. 최초 ping test 진행시 실패 원인 분석.

- CMD창에 __"ping [OpenVPN Server의 퍼브릭 IP]"__ 를 입력하였음.
- "만료된 응답" 이라는 알림이 연속으로 입력됨.
- 검색 결과 퍼블릭 IP로 ping test를 하는경우 그냥 인터넷 망 연결 상태를 확인 한 것임.
- 생성한 OpenVPN EC2는 애초에 퍼블릭IP로의 접근을 차단하도록 설계되었기 때문에 퍼블릭IP로는 접근이 불가능하여 프라이빗 IP로 접속해야함.
- AWS 콘솔 인스턴스에서 OpenVPN EC2의 프라이빗 IP 확인 후, __"ping [OpenVPN Server의 프라이빗 IP]"__ 를 입력, ping test 성공함.

<img width="426" height="167" alt="Image" src="https://github.com/user-attachments/assets/5a6296e9-3584-4881-b064-ecbb44760432" />

