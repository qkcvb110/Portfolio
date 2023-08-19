### 2. RF Communication
> - 전동자동차를 원격으로 조종하기 위한 통신으로 RF 통신을 사용
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
