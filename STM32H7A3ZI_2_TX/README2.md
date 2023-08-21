# 6. Auto Headlight

## **📗 바로가기**

<b>

- [Auto Headlight](#auto-headlight)
- [사용 센서](#사용-센서)
- [FlowChart](#flow-chart)
- [코드 설명](#코드-부분)
- [구현 과정](#구현-과정)

<br/>

## **Auto headlight**
### 밝기에 따른 차량 전조등을 자동으로 제어하는 기능

## **사용 센서**
### **조도 센서**
> ![스크린샷 2023-08-16 210446](https://github.com/qkcvb110/Portfolio/assets/121782690/06bbc9f2-1588-45ec-bf61-8a710f15a41e)
>
> - 현재의 밝기 상태를 측정하기 위한 센서로 사용
> - 조도 센서는 주변의 밝기를 측정하는 센서로 빛 센서라고도 부름
> - 광에너지, 즉 빛을 받으면 전도율이 변하는 소자를 가지고 있으며, 저항을 자유롭게 변화시키는 가변저항으로도 볼 수 있음
> - 조도 센서는 별도의 극성이 존재하지 않으며, 조도센서에 들어오는 빛의 세기와 저항의 크기는 반비례
> - 조도 센서로 측정한 ADC값을 이용하여 차량의 전조등을 PWM을 이용해 자동으로 제어
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/1c487bfe-8c43-4b0d-9ca8-60a0390e294b" width="250" height="350"/>

# **코드 부분**
##  ADC(Analog to Digital Converter) 변환을 시작하는 역할
> - hadc: ADC 핸들(구조체) 포인터이며 이 매개변수를 통해 ADC의 설정과 상태 정보에 접근
> - ADC가 설정된 채널에서 아날로그 변환을 시작
> - 변환은 ADC의 설정에 따라 일련의 샘플링과 변환 과정을 거쳐 이루어짐

```c
HAL_ADC_Start(&hadc1);
```

##  ADC 변환이 완료될 때까지 대기하며, 변환이 완료되면 반환하는 코드
> - 이 함수를 호출하면 함수는 변환이 완료될 때까지 대기
> - 변환이 완료되면 함수가 반환되며, 이후에 변환 결과를 얻음
```c
HAL_ADC_PollForConversion(&hadc1, 10);
```
## 가져온 ADC 값을 프로젝트에 맞춰 변환
> - 전조등을 PWM 제어로 하기 위해 최대 100으로 값을 변경
```c
adc1 = HAL_ADC_GetValue(&hadc1)/650;
```

## 구현 과정
> #### 조도센서 LED제어 모습
> ![KakaoTalk_20230816_174848771_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/812b867f-b1de-473c-9242-8e86f6e7b4b5)
>
