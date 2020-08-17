# 새로운 아이디어 

> 편의성을 향상시키고 보다 위생적으로 체온을 측정하기 위한 IoT

#### 1. 구상 이유

- 현재 SSAFY에서 비접촉식 온도계를 이용해 체온을 측정하지만, 여러 사람의 손을 거쳐 측정하고 있기에 문제점이 있다고 생각
- 체온 측정 후 수기로 정보를 작성하는 불편함과 이를 관리하는 사람의 편의성을 증진시켜야 겠다고 생각

#### 2. 시나리오

1. 관리자가 NFC카드의 정보와 사용자 정보를 DB에 저장
2. NFC 카드를 태그하여 사용자 정보 입력
3. 초음파 거리 센서를 통해 측정하는 사람의 거리 측정
4. 일정 거리에서 사용자 체온 측정 후 자동 저장 & 측정 시간 저장
5. 관리자가 한눈에 측정 정보를 볼 수 있음

#### 3. 사용 장비

- PN-532 (NFC Reader)
- HC-SR04 (Ultrasonic Sensor)
- AMG8833 (Thermal Camera Sensor)
- Rasberry Pi 4 (IoT Module)

#### 4. 사용 기술

- MySQL
- Spring Boot (with Maven, Swagger)
- Vue cli
- PyQt
- Python
- AWS(for deployment)
  - Ubuntu18.04
  - Docker

