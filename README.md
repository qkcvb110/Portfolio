# **PORTFOLIO**

## **📗 목차**

<b>

- 📝 [개요](#-포트폴리오-개요)
- 🛠 [기술 및 도구](#-기술-및-도구)
- ✨ [작품 소개](#-작품-소개)
- 👨🏻‍💻 [기능 구현](#-기능-구현)
  - [CAN,CAN-FD Communication](#1-CAN,CAN-FD_Communication)
  - [RF Communication](#2-RF-Communication)
  - [Lane Keeping Assist](#2-Lane-Keeping-Assist)
  - [Foward Collision-Avoidance Assist](#2-Foward-Collision-Avoidance-Assist)
  - [Blind-Spot Collision-Avoidance Assist](#2-Blind-Spot-Collision-Avoidance-Assist)
  - [Auto headlight](#2-Auto-headlight)
- 🚀 [배포](#-배포)
- ⏰ [커밋 히스토리](#-커밋-히스토리)

</b>

## **📝 포트폴리오 개요**

> **프로젝트:** Smart Communication in ADAS-Equipped Electric Vehicle with CAN-FD, CAN and RF
>
> **기획 및 제작:** 강민성, 오승찬, 최원진
>
> **분류:** Team Project
>
> **제작 기간:** 2023.01 ~ 08.
>
> **주요 기능:** Lane Keeping Assist, Foward Collision-Avoidance Assist, Blind-Spot Collision-Avoidance Assist, Auto headlight, Radio control 
>
> **사용 기술:** CAN-FD, CAN, RF Communication
>
> **문의:** qkcvb110@naver.com

<br />

## **🛠 기술 및 도구**

<img src="https://img.shields.io/badge/STM32-03234B?style=for-the-badge&logo=stmicroelectronics&logoColor=white"> <img src="https://img.shields.io/badge/raspberrypi-A22846?style=for-the-badge&logo=raspberrypi&logoColor=white"> <img src="https://img.shields.io/badge/C-A8B9CC?style=for-the-badge&logo=C&logoColor=white"> <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=Python&logoColor=white"> <img src="https://img.shields.io/badge/GITHUB-181717?style=for-the-badge&logo=github&logoColor=white"> 

<br />

## **✨ 작품 소개**
> **작품 이름:** Smart Communication in ADAS-Equipped Electric Vehicle with CAN-FD, CAN and RF
>
> **작품 목표:** CAN-FD, CAN, RF 통신을 사용하여 ADAS가 탑재된 유무선 전동자동차 제작
>
> **작품 선정 동기:** 팀원 전원이 자동차에 관심이 많아 제작하게 됨 ---수정---
> 
> **목표 기능:** Lane Keeping Assist, Foward Collision-Avoidance Assist, Blind-Spot Collision-Avoidance Assist, Auto headlight, Radio control
>
> **구상도**
> <img width="100%" src=https://github.com/qkcvb110/Portfolio/assets/121782690/39f393b4-c283-44d2-8740-e597be9d826e/>
> **완성 모습**
> ----추가----
>
<br />

## **👨🏻‍💻 기능 구현**

### [**1. CAN,CAN-FD Communication**](https://github.com/qkcvb110/Portfolio/tree/master/STM32H7A3ZI_1_RX)
> - 프로젝트에 사용된 주 통신으로 CAN(Controller Area Network)통신을 사용
> - STM32 간은 CAN-FD, Raspberry Pi와 STM 간의 통신은 CAN을 사용하여 통신
>
> STM32 - Raspberry Pi CAN Communication Test
> 
> ![can_test_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/dafc5989-2f69-422b-8a81-247e9ec70b2f)


### [**2. RF Communication**](https://github.com/qkcvb110/Portfolio/tree/master/STM32F411_TX)
> - 전동자동차를 원격으로 조종하기 위한 통신으로 RF 통신을 사용
> - 조종기로는 핸들과 엑셀레이터를 직접제작하여 가변저항의 ADC값을 RF로 통신
>  
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/ffb32203-ef52-4cc4-9996-8100f3610fce.png" width="300" height="400"/>
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/002638d1-3e93-49a4-9d98-891ba6908acc" width="700" height="400"/>
>
> #### STM32F411에서 ADC값 측정후 STM32(1)로 RF통신을 사용하여 값을 보냄
>
> ![가변저항_측정_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/217bcf46-4593-4650-97a5-0ed26d1d758d)
> 
> #### STM32H7(Rx)-STM32F411(Tx) RF통신으로 ADC값을 이용해 앞뒤모터제어
>
> ![스위치로_앞뒤모터_제어_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/03f7602a-aef4-4f92-900c-84ed59ca76dd)
![핸들_인식_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/1dd06a80-d61e-46d1-bf7e-fd4f82a17426)


### **3. Lane Keeping Assist**
> - ADAS의 기능 중 차선보조기능을 선택하여 구현
> - 보드는 Raspberry Pi 4B 8GB 모델을 선택하여 실시간 데이터를 STM32와 CAN 통신으로 전송
> 
> #### 기능 구현 후 첫 테스트
>
> ![차선인식테스트_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/75f805c7-3357-412b-9420-d1ffd46ef9eb)
> 
> #### 직진 차선 인식테스트
>
> ![직진_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/9f3dbd1c-e622-4752-bfe8-0087a32853cf)
>
> #### 오류값 확인 후 개선 후 최종 
>
> ![차선인식_우회전_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/8905004f-a68d-4041-b79e-c9f64a960ff0)
 ![차선인식_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/7245e01e-ee5e-436e-9c34-db893c3bc78b)

### **4. Foward Collision-Avoidance Assist**
> - 전방 충돌방지 보조 기능 구현
> - LIDAR 센서를 활용해 전방의 물체와의 거리 측정후 일정 거리가 되면 모터 정지
> 
> #### LIDAR CAN-FD 통신
>
> ![라이다_rx_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/be592a73-d340-4922-913a-26ee13bdb912) ![라이다_tx_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/ee5b5969-6770-4355-a4d8-546fede506e4)
>  
> #### LIDAR 테스트 영상
>
> ![라이다_테스트_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/600dd67f-8e80-4219-93ed-bef1ae5e63d0)
>


### **5. Blind-Spot Collision-Avoidance Assist**
> - 후측방 충돌방지 보조 기능 구현
> - 양측면과 후방에 초음파센서 하나씩 부착 후 거리 측정후 일정 거리가 되면 부저경고
> #### 후측면 초음파 센서 부착 모습
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/1b345b4a-867b-4a61-aace-a672f441fa3e" width="300" height="400"/> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/40b30eea-344d-4474-af3e-52f0622c4878" width="300" height="400"/> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/f6496ba3-8ade-4fc1-994e-c4b32bd94941" width="300" height="400"/>


### **6. Auto headlight**
> - 밝기에 따라 차량 전조등 제어
> - 조도센서의 값을 ADC로 변환 후 CAN-FD로 통신 후 전조등 PWM으로 제어
>
> #### 조도센서 LED제어 모습
> ![KakaoTalk_20230816_174848771_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/812b867f-b1de-473c-9242-8e86f6e7b4b5)
