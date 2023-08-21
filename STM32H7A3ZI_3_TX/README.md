# 4. Foward Collision-Avoidance Assist

## **📗 바로가기**

<b>

- [Foward Collision-Avoidance Assist](#foward-collision-avoidance-assist)
- [사용 센서](#tf-luna-lidar-sensor)
- [FlowChart](#flow-chart)
- [코드 설명](#코드-부분)
- [구현 과정](#구현-과정)

<br/>

## **Foward Collision-Avoidance Assist**
### 충돌 방지 보조 시스템: 전방충돌 경고 기능에서 한 단계 더 나아가, 단순 경고뿐만 아니라 차량의 제동까지 직접 보조하는 기능
![HyAnc97QDwSBtT93gcakbvsx2L3MpiN771YD0ZaLfraXffZyGWCj9iBpdHxZ3dYl_uFkzlHz1D9FkEBTBc9npQ_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/7e52c0e3-1b79-4b60-9137-22bcc24fb3dc)

<br/>

## **TF-Luna Lidar Sensor**
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/5d20d854-fddf-4648-a433-8d3a10a4e279" width="250" height="200"/>
 <img width="271" alt="스크린샷 2023-08-21 185837" src="https://github.com/qkcvb110/Portfolio/assets/121782690/b81c782e-b9ac-4557-9e8d-05b6f524c376">
 
> - 차량의 전방의 장애물과의 거리를 측정하기 위한 센서로 사용
> - Lidar 센서로 측정 후 값을 CAN-FD으로 통신하여 모터 제어

## **사용 이유**
> - LiDAR 기술: TF-Luna는 Time-of-Flight (ToF) LiDAR 기술을 사용하여 거리 측정을 수행하는 이 기술은 빛의 시간을 측정하여 거리를 계산하므로 정확한 거리 측정이 가능
> - 높은 정확도: ToF LiDAR 기술은 초음파 센서보다 높은 정확도를 가지며, 정확한 거리 및 위치 측정이 필요한 응용 프로그램에 적합
> - 빠른 응답 속도: ToF LiDAR는 빛의 속도를 기반으로 하기 때문에 빠른 응답 속도를 가지며 이로 인해 빠르게 변하는 환경에서도 정확한 측정이 가능
> - 장거리 측정: TF-Luna 모델은 장거리 측정이 가능하여 초음파 센서보다 더 넓은 범위에서 사용(0.2M ~ 8M 측정가능)

<br/>

## **Flow Chart**
<img width="=350" alt="스크린샷 2023-08-21 191857" src="https://github.com/qkcvb110/Portfolio/assets/121782690/49617af0-b786-4d20-b7df-3ce5ba7b1c3e">


<br/>

# **코드 부분**
## 센서 초기화 및 I2C 주소 설정
#### 이 함수는 TF-Luna 라이더 센서의 구조체에 I2C 핸들과 주소 정보를 설정하여 초기화하는 역할
> - TF_Luna_Lidar *tf_luna: TF_Luna_Lidar 구조체 포인터 매개변수이며 이 매개변수를 통해 함수 외부에서 생성된 구조체에 접근하고 변경
> - I2C_HandleTypeDef *i2c: I2C 통신에 사용되는 핸들을 가리키는 포인터 매개변수
> - TF-Luna 센서의 I2C 주소를 설정
> - tf_luna->TF_Luna_address = TF_Luna_address;: 구조체의 TF_Luna_address 멤버에 함수 매개변수로 받은 TF_Luna_address 값을 할당 후 I2C 주소에 접근

```c
bool TF_Luna_init(TF_Luna_Lidar *tf_luna,I2C_HandleTypeDef *i2c,uint8_t TF_Luna_address)
{

	  tf_luna->i2c = i2c;
	  tf_luna->TF_Luna_address=TF_Luna_address;
	  return 1;

}

```
## 데이터 추출
#### 이 함수는 TF-Luna 라이더 센서에서 데이터를 가져와서 거리, 플럭스, 온도 값을 계산하고 평가하는 역할
> - 에러 상태를 초기화하고 센서 상태를 준비 상태로 설정
> - FL_DIST_LO에서 TFL_TEMP_HI까지의 레지스터를 순회하며 데이터 읽기 후 배열(dataArray)에 저장
> - 읽은 데이터를 변수에 저장
> - dist 변수에 dataArray의 첫 번째 값과 두 번째 값의 시프트 연산 결과를 더하여 저장
> - flux 변수에 dataArray의 세 번째 값과 네 번째 값의 시프트 연산 결과를 더하여 저장
> - temp 변수에 dataArray의 다섯 번째 값과 여섯 번째 값의 시프트 연산 결과를 더하여 저장
> - temp 변수의 값을 100으로 나눠서 섭씨 온도를 백분율로 변환
> - 비정상 데이터 평가한 후 조건이 맞지 않으면 false를 반환, 맞으면 true를 반환

```c
bool getData(TF_Luna_Lidar *tf_luna, int16_t *dist, int16_t *flux, int16_t *temp)
{
    tfStatus = TFL_READY;    // clear status of any error condition

    // - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    // Step 1 - Use the `HAL_I2C_MASTER_Receive` function `readReg` to fill the six byte
    // `dataArray` from the contiguous sequence of registers `TFL_DIST_LO`
    // to `TFL_TEMP_HI` that declared in the header file 'tfluna_i2c.h`.
    // - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    for (uint8_t reg = TFL_DIST_LO; reg <= TFL_TEMP_HI; reg++)
    {
      if( !readReg(tf_luna, reg)) return false;
          else dataArray[ reg] = regReply;
    }

    // - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    // Step 2 - Shift data from read array into the three variables
    // - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
   *dist = dataArray[ 0] + ( dataArray[ 1] << 8);
   *flux = dataArray[ 2] + ( dataArray[ 3] << 8);
   *temp = dataArray[ 4] + ( dataArray[ 5] << 8);



    // Convert temperature from hundredths
    // of a degree to a whole number
   *temp = *temp / 100;
  //  *temp = *temp * 9 / 5 + 32;
    // Then convert Celsius to degrees Fahrenheit


    // - - Evaluate Abnormal Data Values - -
    // Signal strength <= 100
    if( *flux < (int16_t)100)
    {
      tfStatus = TFL_WEAK;
      return false;
    }
    // Signal Strength saturation
    else if( *flux == (int16_t)0xFFFF)
    {
      tfStatus = TFL_STRONG;
      return false;
    }
    else
    {
      tfStatus = TFL_READY;
      return true;
    }

}
```
## **구현 과정**

> #### LIDAR CAN-FD 통신
>
> ![라이다_rx_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/be592a73-d340-4922-913a-26ee13bdb912) ![라이다_tx_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/ee5b5969-6770-4355-a4d8-546fede506e4)
>  
> #### LIDAR 테스트 영상
>
> ![라이다_테스트_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/600dd67f-8e80-4219-93ed-bef1ae5e63d0)
