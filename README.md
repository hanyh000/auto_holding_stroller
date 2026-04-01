오토홀딩 스마트 유모차 (Auto-Holding Smart Stroller)
"보호자의 손이 떨어지면 스스로 멈추는 지능형 안전 유모차" > 압력 센서를 통한 자동 브레이크와 온습도·GPS 기반의 통합 모니터링 시스템

1. 프로젝트 개요
• 배경: 최근 5년간 유모차 사고 중 66.2%가 추락 사고이며, 이는 보호자의 부주의나 경사로 밀림에서 시작됩니다.
• 목적: 보호자의 손 이탈을 실시간 감지하여 즉각적인 자동 브레이크를 작동시키고, 앱을 통해 주변 환경 정보를 제공하여 사고를 예방합니다.
• 기간: 2024.04 ~ 2024.11 (8개월, 한이음 ICT멘토링)
• 역할: 하드웨어 회로 설계, 펌웨어(Arduino/RPi) 구현, 블루투스 연동 앱 제작

2. 핵심 기능
기능상세                 설명
자동 브레이크	          Flex 압력 센서로 손 이탈 감지 시 리니어 모터가 즉시 바퀴 고정
환경 모니터링	          온습도·조도 센서 데이터를 앱으로 전송, 임계값 초과 시 앱 배경색 변경 경고
스마트 팬 제어           온도 24°C 초과 시 자동 작동 및 앱을 통한 수동 제어 모드 지원
실시간 위치 추적         GPS 데이터를 파싱하여 앱 내 구글맵 링크로 유모차 현재 위치 확인
데이터 로깅              모든 센서 이력을 라즈베리파이 내 SQLite DB에 저장하여 관리

3. 시스템 아키텍처
Hardware Connection
• Master: Raspberry Pi 4 (Data Hub & DB)
• Slaves: Arduino Uno (x4) - 각 센서 및 액추에이터 제어
• Interface: Serial (USB-UART), Bluetooth 2.0 (HC-06)

4. 기술 스택 및 부품
Tech Stack
• Languages: Python (RPi Main), C++ (Arduino Sketch)
• Database: SQLite3
• App: MIT App Inventor
• Communication: Serial (USB), UART (GPS), Bluetooth

Components
• Controller: Raspberry Pi 1, Arduino Board 4
• Sensors: Flex (Pressure), DHT11 (Temp/Humid), CDS (Light), GPS Module, Ultrasonic
• Motors: Linear Motor (Brake), DC Motor (Fan)

5. 프로젝트 구조
'''
├── stroller_gps_sensor_db.py   # 메인 컨트롤러 (Multi-threading, DB, Comm)
├── dht_DC.ino                  # 온습도 측정 및 선풍기 제어
├── CDS.ino                     # 조도 데이터 전송
├── Linear_motor.ino            # 압력 센서 기반 브레이크 로직 (핵심)
├── ultrasonic.ino              # 장애물 감지 보조
└── not_control_SERVO.aia       # MIT App Inventor 프로젝트 파일
'''

7. 주요 기술적 해결 (Troubleshooting)
1. 센서 노이즈 및 간섭 해결
• 문제: DC 모터 작동 시 발생하는 진동이 온습도 센서 값에 영향을 주는 현상 발생.\
• 해결: 모터와 센서의 물리적 마운트를 분리하고, 모터를 탄성 있는 소재로 고정하여 진동 전달 차단.

2. 데이터 패킷 유실 방지 (데이터 끝점 현상)
• 문제: 시리얼 통신 시 여러 센서 데이터가 뒤섞여 수신되는 현상.
• 해결: 패킷 앞뒤에 시작(11)과 끝(00\n) 마커를 추가한 커스텀 프로토콜을 설계하여 데이터 무결성 확보.

7. 결과 및 시연
• 시연 영상: youtube.com/watch?v=IndQpyns9EA&source_ve_path=OTY3MTQ&embeds_referring_euri=https%3A%2F%2Fwww.notion.so%2F
• 앱 인터페이스: * 조도/온도/습도에 따른 동적 배경색 변경 로직 적용
  • 실시간 GPS 좌표를 기반으로 한 구글맵 연동 기능
