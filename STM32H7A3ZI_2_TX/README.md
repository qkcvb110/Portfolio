# 5. Blind-Spot Collision-Avoidance Assist

## **📗 바로가기**

<b>

- [후측방 충돌방지 보조 기능](#blind-spot-collision-avoidance-assist)
- [사용한 센서](#사용한-센서)
- [코드 설명](#코드-부분)
- [구현 과정](#구현-과정)

<br/>

## **Blind-Spot Collision-Avoidance Assist**
### 후측방 충돌방지 보조 기능: 후측방 사각지대에 다른 차량이 있을 때 경고음을 들려주는 기능
<img width="400" alt="스크린샷 2023-08-21 200209" src="https://github.com/qkcvb110/Portfolio/assets/121782690/7dba4fa1-de90-432f-b4f2-cc7433a50240">

## **사용한 센서**
### **초음파 센서**
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/c48a83d8-6d87-4685-8cce-663bbd5ef7c1" width="350" height="200"/>

> - 차량의 후측방의 장애물과의 거리를 측정하기 위한 센서로 사용
### **초음파 센서의 원리**
<img width="547" alt="image" src="https://github.com/qkcvb110/Portfolio/assets/121782690/9f79e3d2-4db6-4411-9fb0-7ff11ea0fe6d">

> - 초음파 센서란 초음파를 이용해 앞에 있는 사물간의 거리를 인식하는 센서
> - 초음파 센서는 Trig, Echo로 나뉨
> - 작동 원리로는 Trig는 초음파를 보내는 부분이고, Echo는 Trig에서 나온 초음파가 사물에 부딪혀 돌아오는 시간을 읽음으로써 거리를 측정하는 원리

<br/>

#### **부저(LED)**

<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/dd24b98c-69e6-4768-aaab-9fde7b29cf26" width="200" height="200"/>
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/aae0fab5-ae22-49d8-bdb4-f9515fb705f4" width="100" height="200"/>

> - 초음파센서로 측정한 거리를 이용해 일정거리 이하일 경우 부저(LED)를 이용해 운전자에게 경고
> - GITHUB에는 소리를 표현할 수 없어 LED 자료로 업로드

<br/>

# **코드 부분**
## 이 코드는 입력 신호의 간격을 측정하고 이를 이용하여 거리를 계산하는 프로세스를 구현
> - 이 프로젝트에선 후측방에 각각 하나씩, 총 세개의 초음파를 사용
> - 그에 맞게 세개의 타이머가 필요함으로써 각 타이머 채널 1,3,4를 사용

#### 코드 설명
> - 입력 캡처 인터럽트가 발생한 채널이 n번 채널인지 확인
> - 캡처된 값이 있다면 읽어온 후 다음 캡처를 기다리기 위해 채널 n의 입력 펄스폴링을 하강 엣지로 설정합니다.
> - 이미 첫번째 값이 캡처된 경우 두번째 캡처된 값을 읽어온 후 타이머 카운터를 리셋
> - 캡처된 두 값의 차이를 계산(오버플로우되는 경우도 처리)
> - 측정된 간격을 기반으로 거리를 계산
> - 첫번째 값을 다시 캡처해야하므로 플래그를 초기화
> - 다음 캡처를 위해 채널 n의 입력 펄스 폴링을 상승 엣지로 설정
> - 캡처 인터럽트를 비활성화

```c
void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)
{
	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1)  // if the interrupt source is channel1
	{
		if (Is_First_Captured_1==0) // if the first value is not captured
		{
			IC_Val1_1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1); // read the first value
			Is_First_Captured_1 = 1;  // set the first captured as true
			// Now change the polarity to falling edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING);
		}

		else if (Is_First_Captured_1==1)   // if the first is already captured
		{
			IC_Val2_1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2_1 > IC_Val1_1)
			{
				Difference_1 = IC_Val2_1-IC_Val1_1;
			}

			else if (IC_Val1_1 > IC_Val2_1)
			{
				Difference_1 = (0xffff - IC_Val1_1) + IC_Val2_1;
			}

			Distance_1 = Difference_1 * .034/2;
			Is_First_Captured_1 = 0; // set it back to false

			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
			__HAL_TIM_DISABLE_IT(&htim3, TIM_IT_CC1);
		}
	}

	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_2)  // if the interrupt source is channel1
	{
		if (Is_First_Captured_2==0) // if the first value is not captured
		{
			IC_Val1_1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_2); // read the first value
			Is_First_Captured_2 = 1;  // set the first captured as true
			// Now change the polarity to falling edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_2, TIM_INPUTCHANNELPOLARITY_FALLING);
		}

		else if (Is_First_Captured_2==1)   // if the first is already captured
		{
			IC_Val2_2 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_2);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2_2 > IC_Val1_2)
			{
				Difference_2 = IC_Val2_2-IC_Val1_2;
			}

			else if (IC_Val1_2 > IC_Val2_2)
			{
				Difference_2 = (0xffff - IC_Val1_2) + IC_Val2_2;
			}

			Distance_2 = Difference_2 * .034/2 - 40;
			Is_First_Captured_2 = 0; // set it back to false

			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_2, TIM_INPUTCHANNELPOLARITY_RISING);
			__HAL_TIM_DISABLE_IT(&htim3, TIM_IT_CC2);
		}
	}




	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_4)  // if the interrupt source is channel1
			{
				if (Is_First_Captured_4==0) // if the first value is not captured
				{
					IC_Val1_4 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_4); // read the first value
					Is_First_Captured_4 = 1;  // set the first captured as true
					// Now change the polarity to falling edge
					__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_4, TIM_INPUTCHANNELPOLARITY_FALLING);
				}

				else if (Is_First_Captured_4==1)   // if the first is already captured
				{
					IC_Val2_4 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_4);  // read second value
					__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

					if (IC_Val2_4 > IC_Val1_4)
					{
						Difference_4 = IC_Val2_4-IC_Val1_4;
					}

					else if (IC_Val1_4 > IC_Val2_4)
					{
						Difference_4 = (0xffff - IC_Val1_4) + IC_Val2_4;
					}

					Distance_4 = Difference_4 * .034/2;
					Is_First_Captured_4 = 0; // set it back to false

					// set polarity to rising edge
					__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_4, TIM_INPUTCHANNELPOLARITY_RISING);
					__HAL_TIM_DISABLE_IT(&htim3, TIM_IT_CC4);
				}
			}


}
```

## 이 함수는 초음파 센서를 사용하여 거리를 측정하기 위한 동작의 시작 단계를 수행
> - 하나의 초음파 센서를 사용하여 거리를 측정하는 함수
> - 초음파를 생성하기 위해 TRIG 핀을 HIGH 상태로 유지한 후 10 마이크로초 동안 대기, 이 시간은 초음파의 파형이 발생하고 되돌아오는 시간을 고려하여 설정
> - 초음파 파형을 생성하기 위해 TRIG 핀을 HIGH로 설정, 이로써 초음파가 발생하도록 준비
> - 초음파 파형을 생성하기 위해 TRIG 핀을 다시 LOW로 설정, 이로써 초음파 파형의 발생이 종료
> - 타이머 인터럽트 채널 1 (CC1)을 활성화하여 입력 캡처를 위한 인터럽트를 가능(초음파의 에코 신호가 센서로부터 다시 수신되는 시간을 측정하기 위해 사용)
> - 위와 같은 방법으로 3개의 초음파 센서를 활성화

```c
void HCSR04_Read1 (void)
{
   HAL_GPIO_WritePin(TRIG_PORT1, TRIG_PIN1, GPIO_PIN_SET);  // pull the TRIG pin HIGH
   delay(10);  // wait for 10 us
   HAL_GPIO_WritePin(TRIG_PORT1, TRIG_PIN1, GPIO_PIN_RESET);  // pull the TRIG pin low

   __HAL_TIM_ENABLE_IT(&htim3, TIM_IT_CC1);

}
```


## **구현 과정**
#### **후측면 초음파 센서 부착 모습**

> 
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/1b345b4a-867b-4a61-aace-a672f441fa3e" width="300" height="400"/> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/40b30eea-344d-4474-af3e-52f0622c4878" width="300" height="400"/> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/f6496ba3-8ade-4fc1-994e-c4b32bd94941" width="300" height="400"/>

<br/>


