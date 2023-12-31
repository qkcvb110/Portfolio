# 6. Motor

## **📗 바로가기**

<b>

- [사용 모듈 및 모터](#사용-모듈-및-모터)
- [FlowChart](#flow-chart)
- [코드 설명](#코드-부분)
- [구현 과정](#구현-과정)

<br/>

## **사용 모듈 및 모터**
> - 모터는 앞바퀴 12V DC모터한개와 뒷바퀴 6V DC모터 두개를 사용
> - 앞바퀴,뒷바퀴 모두 힘을 늘리기 위한 기어박스와 함께 장착
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/af2d0f0e-2e6d-47b4-87fd-833e0b0e1af8" width="250" height="200"/>

> - 모터 드라이버는 2CH 하나와 1CH 하나를 사용
> - 각각 뒷바퀴 두개와 앞바퀴 한개씩 사용
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/3c42a6b3-04a1-4ed8-86bf-98afa5bce165" width="500" height="200"/>

<br/>

## **Flow Chart**
> - 모터는 RF통신과 CAN,CAN-FD통신을 받아 작동
> - RF통신으로는 제작한 핸들과 엑셀레이터, 두개의 스위치의 값을 STM32F411을 통해 원격통신하여 수신받은 값을 이용
> - CAN,CAN-FD통신으로는 라즈베리파이 차선인식의 CAN, STM32 Lidar의 CAN-FD의 값을 이용
![스크린샷 2023-08-22 222851](https://github.com/qkcvb110/Portfolio/assets/121782690/28d08fe1-21da-4def-9d37-1186c8e55287)

<br/>

# **코드 부분**
## 전후진을 변경하는 스위치와 엑셀레이터로 모터를 제어하는 함수
> - ```RxData[2]``` 는 전후진을 위한 스위치이며
> - ```lidar();``` 는 전방의 거리를 측정하여 일정거리 이하이면 모터가 정지하는 함수
> - ```RxData[0]``` 는 엑셀레이터의 가변저항 ADC값이며 가끔 100을 살짝 초과하는 경우가 있어 그 이상일 경우 CCR을 99로 제한을 두었음
> - ```light_sensor();``` 는 조도센서의 ADC값을 받아 차량의 전조등의 밝기를 PWM에 적용해 자동으로 제어하는 함수
```c
void go_back(void)
{
  if(RxData[2]==0)
  {
    HAL_GPIO_WritePin(GPIOE, GPIO_PIN_14, 0); //뒷모터 후진
		HAL_GPIO_WritePin(GPIOA, GPIO_PIN_3,0); //뒷모터 후진
		htim1.Instance->CCR1=RxData[0]; //CCR1를 엑셀레이터값(RxData[0])으로 설정
		htim1.Instance->CCR2=RxData[0]; //CCR2를 엑셀레이터값(RxData[0])으로 설정
		if(RxData[0]>=100)
		{
      htim1.Instance->CCR1=99;
      htim1.Instance->CCR2=99;
    }
    light_sensor();
    lidar();
  }
  if(RxData[2]==1)
  {
    HAL_GPIO_WritePin(GPIOE, GPIO_PIN_14, 1); //뒷모터 전진
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_3,1); //뒷모터 전진
    htim1.Instance->CCR1=RxData[0]; //CCR1를 엑셀레이터값(RxData[0])으로 설정
    htim1.Instance->CCR2=RxData[0]; //CCR2를 엑셀레이터값(RxData[0])으로 설정
    if(RxData[0]>=100)
    {
      htim1.Instance->CCR1=99;
      htim1.Instance->CCR2=99;
    }
    light_sensor();
    lidar();
  }
}
```
### 결과 영상
>![뒷바퀴 엑셀](https://github.com/qkcvb110/Portfolio/assets/121782690/c9771932-f7c3-4f74-92a0-41dc8a3f20c6)
<br/>

## 핸들의 가변저항을 이용한 모터 제어와 스위치를 이용한 모드 변경 함수
> - ```RxData[3]==0```을 핸들 제어 모드
> - ```RxData[1]``` 이 50보다 작을때와 65보다 클때를 나눔
> - ```RxData[1]  < 50```일때 동안 앞바퀴를 왼쪽으로 제어
> - ```RxData[1]  > 65```일때 동안 앞바퀴를 오른쪽으로 제어
> - ```if(50<=RxData[1]&&RxData[1]<=65)``` 를 이용하여 핸들을 한쪽으로 틀었다가 다시 중앙으로 왔을때 반대방향으로 모터를 제어하여 핸들을 중앙으로 위치시킴 
> - ```light_sensor();```는 위와 동일
> - ```buzzer();```는 STM32H7A3ZI_2_TX의 초음파값이 일정 거리 이하일때 부저로 경고음
> - CCR값이 다른 이유는 모터의 회전방향에 따라 CCR값에 따른 힘이 달라 힘을 맞추기 위해 다르게 설정
```c
void nrf_motor (void)
{
  if(RxData[3]==0) 
	{
    if (RxData[1]  <50)  
    {
      while (RxData[1]  < 50)  
      {
        light_sensor();
        buzzer();
        if (isDataAvailable(2) == 1)
        {
          NRF24_Receive(RxData);
        }
        htim1.Instance->CCR3 = 80;
        HAL_GPIO_WritePin(GPIOG, GPIO_PIN_14, 0);
        HAL_Delay(100);
        go_back();
        if(50<=RxData[1]&&RxData[1]<=65)
        {
          htim1.Instance->CCR3 = 100;
          HAL_GPIO_WritePin(GPIOG, GPIO_PIN_14, 1);
          HAL_Delay(300);
          htim1.Instance->CCR3 = 0;
        }
      }
    }
    if (RxData[1] > 65)
    {
      while (RxData[1] > 65)
      {
        light_sensor();
        buzzer();
        if (isDataAvailable(2) == 1)
        {
          NRF24_Receive(RxData);
        }
        htim1.Instance->CCR3 = 80;
        HAL_GPIO_WritePin(GPIOG, GPIO_PIN_14, 1);
        HAL_Delay(100);
        go_back();
        if(50<=RxData[1]&&RxData[1]<=65)
        {
          htim1.Instance->CCR3 = 90;
          HAL_GPIO_WritePin(GPIOG, GPIO_PIN_14, 0);
          HAL_Delay(250);
          htim1.Instance->CCR3 = 0;
        }
      }
    }
  }
}
```

### 결과 영상
![](https://github.com/crasdok/capstone/assets/118472691/4d100a40-bb1d-4ec3-a9a1-659828b5c2a6)

<br/>

## 라즈베리파이의 차선인식을 통해 진행방향을 이용하여 모터 제어 함수
> - ```RxData[3]==1```을 차선보조주행 모드
> - 아래의 코드도 위와 같은 알고리즘으로 코드를 구현
> - ```RxData_From_Node4[0]=='L'```일때 동안 앞바퀴를 왼쪽으로 제어
> - ```RxData_From_Node4[0]=='R'```일때 동안 앞바퀴를 오른른쪽으로 제어
> - ```if(RxData_From_Node4[0]=='G')```를 이용하여 차선이 Left 또는 right 로 핸들이 한쪽으로 틀었다가 Go 로 다시 왔을때 반대방향으로 모터를 제어하여 핸들을 중앙으로 위치시킴
> - 마찬가지로  ```light_sensor();```,```buzzer();```는 위와 동일
> - CCR값이 다른이유도 위와 동일
```c
void rpi_motor (void)
{
	if(RxData[3]==1)
	{
		while (RxData_From_Node4[0]=='L')
		{
			light_sensor();
			buzzer();
			if (isDataAvailable(2) == 1)
			{
				NRF24_Receive(RxData);
			}
			htim1.Instance->CCR3 = 80;
			HAL_GPIO_WritePin(GPIOG, GPIO_PIN_14, 0);
			HAL_Delay(100);
			go_back();
			if(RxData[3]==0)
			{
				break;
			}
			if(RxData_From_Node4[0]=='G')
			{
				htim1.Instance->CCR3 = 100;
				HAL_GPIO_WritePin(GPIOG, GPIO_PIN_14, 1);
				HAL_Delay(200);
				htim1.Instance->CCR3 = 0;
				break;
			}
		}
		while (RxData_From_Node4[0]=='R')
		{
			light_sensor();
			buzzer();
			if (isDataAvailable(2) == 1)
			{
				NRF24_Receive(RxData);
			}
			htim1.Instance->CCR3 = 80;
			HAL_GPIO_WritePin(GPIOG, GPIO_PIN_14, 1);
			HAL_Delay(100);
			go_back();
			if(RxData[3]==0)
			{
				break;
			}
			if(RxData_From_Node4[0]=='G')
			{
				htim1.Instance->CCR3 = 90;
				HAL_GPIO_WritePin(GPIOG, GPIO_PIN_14, 0);
				HAL_Delay(200);
				htim1.Instance->CCR3 = 0;
				break;
			}
		}
	}
}
```
## Lidar 센서로 거리를 측정 후 일정거리 이하 일때 모터 정지
> - Lidar로 측정한 거리가 100의 자리, 10의 자리, 1의 자리로 나뉘어져있어 ASCII 코드를 10진수로 변환후 합쳐 Distance4값으로 적용
> - 측정거리가 20cm 이하일 경우 모터가 정지
```c
void ridar (void)
{
	for(int j=0;j<3;j++)
	{
		b[j]=RxData_From_Node1[j]-'0';
	}
	int Distance4 = 100* b[0]  +10*b[1] +b[2];//rider
	if(Distance4<=20)
	{
		htim1.Instance->CCR1=0;
		htim1.Instance->CCR2=0;
	}
}
```

## 부저를 이용하여 모든 초음파의 거리가 15cm 이하일때 경고음 작동
> - Lidar와 마찬가지로 측정한 거리가 100의 자리, 10의 자리, 1의 자리로 나뉘어져있어 ASCII 코드를 10진수로 변환후 합쳐 각각 Distance1,2,3값으로 적용
```c
void buzzer (void)
{
	for(int i=0;i<11;i++)
	{
		a[i]=RxData_From_Node3[i]-'0';
	}
	int Distance1 = 100* a[1]  +10*a[2] +a[3];
	int Distance2 = 100* a[4]  +10*a[5] +a[6];
	int Distance3 = 100* a[8]  +10*a[9] +a[10];
	if (Distance1 <= 15 || Distance2 <= 15 || Distance3 <= 15)
	{
		TIM2->ARR = C;
		TIM2->CCR1 = TIM2->ARR / 2;
		HAL_Delay(50);
		TIM2->CCR1 = 0;
		HAL_Delay(50);
	 }
}
```

## 조도센서를 통해 받은 ADC값을 PWM으로 적용후 전조등LED 제어
> - Lidar,초음파와 마찬가지로 수신받은 ADC 값이 100의 자리, 10의 자리, 1의 자리로 나뉘어져있어 ASCII 코드를 10진수로 변환후 합쳐 값으로 적용
```c
void light_sensor (void)
{
	for(int l=12;l<=14;l++)
	{
		a[l]=RxData_From_Node3[l]-'0';
	}
	int jodo = 100* a[12]  +10*a[13] +a[14];
	htim3.Instance->CCR1=jodo;
	if (jodo<45)
	{
		htim3.Instance->CCR1=0;
	}
}
```
