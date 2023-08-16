# Project preparation
- 🛠 [사용한 보드 및 센서]
  - [STM32H7A3ZI](STM32H7A3ZI)
  - [RF Communication](#2-RF-Communication)
  - [Lane Keeping Assist](#2-Lane-Keeping-Assist)
  - [Foward Collision-Avoidance Assist](#2-Foward-Collision-Avoidance-Assist)
  - [Blind-Spot Collision-Avoidance Assist](#2-Blind-Spot-Collision-Avoidance-Assist)
  - [Auto headlight](#2-Auto-headlight)
<br />

## **개발 보드**

**STM32H7A3ZI**
>
>![스크린샷 2023-08-16 201614](https://github.com/qkcvb110/Portfolio/assets/121782690/164f51c3-1997-4bc1-8756-14d03c7d57be)
> 
>
> **사용 이유**
>
>- ###### ST 마이크로일렉트로닉스에서 개발한 고성능 ARM Cortex-M7 기반의 마이크로컨트롤러 보드
>- ###### 다양한 임베디드 시스템 및 프로젝트 개발에 사용되고, 다양한 기능과 기능을 갖춘 고성능 보드
>- ###### 프로젝트에서 사용해야할 CAN-FD 또한 지원함으로 STM32H7A3ZI 모델을 선택

<br />

**STM32F411**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/8b0afe9f-aaea-4f0c-8c28-36d22c5bfc7e" width="250" height="200"/>
> 
>
> **사용 이유**
>
>- ###### ST 마이크로일렉트로닉스에서 개발한 고성능 ARM Cortex-M4 기반의 마이크로컨트롤러 보드
>- ###### 다양한 임베디드 시스템 및 프로젝트 개발에 사용되고, 다양한 기능과 기능을 갖춘 고성능 보드
>- ###### 조종기에 부착을 해야하므로 작은 모델로 선택

<br />

**Raspberry Pi 4B 8GB:**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/18df6acc-9094-4b77-9baa-12ee5799498a" width="250" height="200"/>
>
>
> **사용 이유**
>
>- ###### 라즈베리 파이 4B는 라즈베리 파이 재단에서 개발한 널리 사용되는 싱글보드 컴퓨터(Single Board Computer, SBC)
>- ###### 이 보드는 다양한 프로젝트 및 개발 활동에 활용되며, 소형이지만 강력한 기능을 보유
>- ###### 스펙을 바탕으로 Open CV를 활용하여 차선인식을 무리 없이 하기위해 8GB 모델을 선택

<br />

## **사용한 센서**
**CAN Transceiver**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/bed563ab-1427-45f2-bc05-df0f8eeda449" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### STM32H7A3ZI에는 CAN Controller가 내장되어있지만 CAN Transceiver가 내장되어있지않아 사용

<br />

**CAN Bus Controller Module**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/f13da744-fddf-4096-a8e5-9a64286e2b1d" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### Raspberry Pi 4B에는 CAN이 따로 지원하지 않기 때문에 CAN Controller와 CAN Transceiver가 모두 내장되어있는 모듈을 사용

<br />

**NRF24L01**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/c315f0a4-9ab1-4b5b-a595-ffa0a24f8e52" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### RF통신을 사용하여 블루투스보다 안정적으로 통신할 수 있으며 저렴하고 안테나로 인해 먼 거리까지 통신이 가능

<br />

**Ultrasonic Sensor**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/d5fa731f-2f6c-4179-ac16-ec4af578e5a1" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### 후측방 장애물과의 거리를 측정하기 위한 센서로 사용

<br />

**LIDAR**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/f5dd8dbe-e0fd-4271-900b-abd8cb729690" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### 전방 장애물과의 거리를 측정하기 위한 센서로 사용

<br />

**조도 센서**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/3fa11c9c-f80b-48aa-a28e-35b46de3a448" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### 밝기를 이용해 자동 전조등을 구현하기위해 사용

<br />

**부저**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/45541ab4-bc5c-4a19-a0b7-d6955049db5d" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### 초음파의 측정거리가 일정거리 미만일때 부저 경고음

<br />

**가변저항**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/3792214a-f626-47db-b75c-a821e346d2e8" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### 핸들과 엑셀레이터에 가변저항 장착후 ADC값을 측정후 통신

<br />

**모터 드라이버**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/3c42a6b3-04a1-4ed8-86bf-98afa5bce165" width="500" height="200"/>
>
>**사용 이유**
>
>- ###### 전동자동차의 모터를 제어하기위해 차례대로 뒷바퀴2개와 앞방향제어모터 1개씩 사용

<br />

**모터와 기어박스**
>
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/af2d0f0e-2e6d-47b4-87fd-833e0b0e1af8" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### 전동자동차를 움직이기 위해 모터에 기어박스를 장착해 힘 증폭
