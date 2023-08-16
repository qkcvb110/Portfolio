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
  - [Forward Collision Warning](#2-Forward-Collision-Warning)
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
> **주요 기능:** Lane Keeping Assist, Forward Collision Warning, Blind-Spot Collision-Avoidance Assist, Auto headlight, Radio control 
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
> **목표 기능:** Lane Keeping Assist, Forward Collision Warning, Blind-Spot Collision-Avoidance Assist, Auto headlight, Radio control
>
> **구상도**
> <img width="100%" src=https://github.com/qkcvb110/Portfolio/assets/121782690/39f393b4-c283-44d2-8740-e597be9d826e/>

<br />

## **👨🏻‍💻 기능 구현**

### **1. CAN,CAN-FD Communication**

- 프로젝트에 사용된 주 통신으로 CAN(Controller Area Network)통신을 사용
- STM32 간은 CAN-FD, Raspberry Pi와 STM 간의 통신은 CAN을 사용하여 통신

STM32 - Raspberry Pi CAN Communication Test

![can_test_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/dafc5989-2f69-422b-8a81-247e9ec70b2f)

### **2. RF Communication**

- 전동자동차를 원격으로 조종하기 위한 통신으로 RF 통신을 사용
- 조종기로는 핸들과 엑셀레이터를 직접제작하여 가변저항의 ADC값을 RF로 통신
  
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/ffb32203-ef52-4cc4-9996-8100f3610fce.png" width="300" height="400"/>
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/002638d1-3e93-49a4-9d98-891ba6908acc" width="700" height="400"/>

![가변저항_측정_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/217bcf46-4593-4650-97a5-0ed26d1d758d)


- STM32F411에서 ADC값 측정후 STM32(1)로 RF통신을 사용하여 값을 보냄

- STM32H7(Rx)-STM32F411(Tx) RF통신으로 ADC값을 이용해 앞뒤모터제어

![스위치로_앞뒤모터_제어_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/1b341524-85f7-45f4-9da8-7df0815f8bf1) ![핸들_인식_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/bd30c80f-2607-49d8-8700-69eadadf768f)


### **3. Lane Keeping Assist**

### **4. Forward Collision Warning**

### **5. Blind-Spot Collision-Avoidance Assist**

### **6. Auto headlight**
