# 6. Auto headlight

## **📗 바로가기**

<b>

- [Foward Collision-Avoidance Assist](#foward-collision-avoidance-assist)
- [사용 센서](#tf-luna-lidar-sensor)
- [FlowChart](#flow-chart)
- [코드 설명](#코드-부분)
- [구현 과정](#구현-과정)

<br/>

> - 밝기에 따라 차량 전조등 제어
> - 조도센서의 값을 ADC로 변환 후 CAN-FD로 통신 후 전조등 PWM으로 제어
>
> #### 조도센서 LED제어 모습
> ![KakaoTalk_20230816_174848771_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/812b867f-b1de-473c-9242-8e86f6e7b4b5)
>
