# 2. RF Communication

## **📗 바로가기**

<b>

- [RF통신](#RF통신이란)
- [CAN,CAN-FD를 사용한 이유](#사용-이유)
- [사용한 모듈 소개](#사용한-모듈)
- [연결 방법 및 FlowChart](#연결-방법)
- [코드](#코드-설명)

<br/>

## **RF통신이란**
#### RF통신은 이름 그대로 Radio Frequency에서 작동하는 통신이다. 
#### 통신 주파수 대역은 일반적으로 30kHz에서 300GHz이다. RF 통신에선 digital 신호를 전파에 실어서 송신하는데, 전파에 전달하는 다양한 기법이 있다. Amplitude Shift Keying (ASK)와 Pulse Width Modulation (PWM) 등과 같은 다양한 기법을 사용하여 digital신호를 송신한다.

<br/>

## **사용 이유**
> - 여러 통신을 비교해보았을때 블루투스,와이파이,IR 통신, RF를 비교
> - RF 통신은 IR통신에 비교해 더 멀리까지 신호를 전달
> - RF 통신은 송신기와 수신기 사이에 장애물이 있어도 통신이 가능
>   
>  ![스크린샷 2023-08-21 163632](https://github.com/qkcvb110/Portfolio/assets/121782690/eb6bca92-a88e-49b8-8c7b-35ce7a0963c2)
> - RF 통신은 특정 주파수 대역을 사용하므로 간섭의 영향이 낮음
>   
> #### **앞의 이유들로 전동자동차를 원격으로 조종하기 위한 통신 중 RF를 선택**

<br/>

## **사용 모듈**
> ![스크린샷 2023-08-21 164011](https://github.com/qkcvb110/Portfolio/assets/121782690/470771e1-6083-4b66-b669-7b28668bfd3b)
> - RF통신의 모듈 중 안테나가 있는 nRF24l01모듈을 선택하여 통신 거리를 증가







<br/>

> - 조종기로는 핸들과 엑셀레이터를 직접제작하여 가변저항의 ADC값을 RF로 통신
>  
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/ffb32203-ef52-4cc4-9996-8100f3610fce.png" width="300" height="400"/>
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/002638d1-3e93-49a4-9d98-891ba6908acc" width="700" height="400"/>
> #### STM32F411에서 ADC값 측정후 STM32(1)로 RF통신을 사용하여 값을 보냄
>
> ![가변저항_측정_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/217bcf46-4593-4650-97a5-0ed26d1d758d)
> 
> #### STM32H7(Rx)-STM32F411(Tx) RF통신으로 ADC값을 이용해 앞뒤모터제어
>
> ![스위치로_앞뒤모터_제어_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/03f7602a-aef4-4f92-900c-84ed59ca76dd)
![핸들_인식_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/1dd06a80-d61e-46d1-bf7e-fd4f82a17426)
