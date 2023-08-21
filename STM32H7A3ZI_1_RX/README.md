# 1. CAN,CAN-FD Communication
> - 프로젝트에 사용된 주 통신으로 CAN(Controller Area Network)통신을 사용
> - STM32 간은 CAN-FD, Raspberry Pi와 STM 간의 통신은 CAN을 사용하여 통신
 
<br/>

## **📗 바로가기**

<b>

- [CAN,CAN-FD를 사용한 이유](#사용-이유)
- [사용한 모듈 소개](#사용한-모듈)
- [연결 방법 및 FlowChart](#연결-방법)
- [코드](#코드-설명)

### **사용 이유**
> #### 상용화된 자동차에서 사용중인 CAN, CAN-FD를 아래와 같은 이유로 프로젝트에 사용
> #### 멀티플렉스된 통신
> - 다양한 시스템 간의 효율적이고 신속한 데이터 교환을 가능
> #### 고신뢰성 및 내구성
> - 자동차 환경은 진동, 온도 변화, 전압 변동 등의 불안정한 조건을 포함한다. CAN 통신은 이러한 환경에서도 안정적으로 작동하는 고신뢰성과 내구성을 가지고 있음
> #### 데이터 우선순위
> - CAN 통신은 데이터의 우선순위를 설정하여 중요한 데이터에 대한 우선 처리 가능
> #### 단순한 배선
> - CAN 통신은 BUS 통신방법을 사용하여 데이터를 여러 시스템 간에 하나의 통신 선로를 통해 교환할 수 있음
> #### 컴포넌트의 통합 및 모듈화
> - CAN 통신을 사용하면 자동차의 다양한 시스템을 모듈화하여 컴포넌트 간의 통합을 용이

<br/>

### 사용한 모듈
**CAN Transceiver**
> ###### STM32의 CAN-DATA를 보내기 위한 모듈
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/bed563ab-1427-45f2-bc05-df0f8eeda449" width="250" height="200"/>
>
>**사용 이유**
>- ###### STM32H7A3ZI에는 CAN Controller가 내장되어있지만 CAN Transceiver가 내장되어있지않아 사용

<br/>

**CAN Bus Controller Module**
> ###### Raspberry Pi 4B CAN을 사용하기 위한 모듈
><img src="https://github.com/qkcvb110/Portfolio/assets/121782690/f13da744-fddf-4096-a8e5-9a64286e2b1d" width="250" height="200"/>
>
>**사용 이유**
>
>- ###### Raspberry Pi 4B에는 CAN이 따로 지원하지 않기 때문에 CAN Controller와 CAN Transceiver가 모두 내장되어있는 모듈을 사용

<br/>

### **연결 방법**

![image](https://github.com/qkcvb110/Portfolio/assets/121782690/130d3b31-17a0-4d8d-b27c-d326193fdab8)

<br/>

### Flow Chart
![image](https://github.com/qkcvb110/Portfolio/assets/121782690/2d791928-7da1-4d33-ae4a-072677429d10)




# 코드 설명
### CAN 초기화 및 설정
**이 코드는 STM32 HAL (Hardware Abstraction Layer) 라이브러리를 사용하여 FDCAN1 (Flexible Data Rate Controller Area Network 1)을 초기화하는 함수이며 CAN-FD은 CAN 통신을 지원하는 하드웨어 모듈로, 고속 데이터 전송 및 에러 검출 기능을 포함**
```c
    void MX_FDCAN1_Init(void)
    {
        hfdcan1.Instance = FDCAN1;
        hfdcan1.Init.FrameFormat = FDCAN_FRAME_FD_NO_BRS;
        hfdcan1.Init.Mode = FDCAN_MODE_NORMAL;
        hfdcan1.Init.AutoRetransmission = ENABLE;
        hfdcan1.Init.TransmitPause = DISABLE;
        hfdcan1.Init.ProtocolException = DISABLE;
        hfdcan1.Init.NominalPrescaler = 1;
        hfdcan1.Init.NominalSyncJumpWidth = 1;
        hfdcan1.Init.NominalTimeSeg1 = 5;
        hfdcan1.Init.NominalTimeSeg2 = 2;
        hfdcan1.Init.DataPrescaler = 1;
        hfdcan1.Init.DataSyncJumpWidth = 4;
        hfdcan1.Init.DataTimeSeg1 = 5;
        hfdcan1.Init.DataTimeSeg2 = 4;
        hfdcan1.Init.MessageRAMOffset = 0;
        hfdcan1.Init.StdFiltersNbr = 1;
        hfdcan1.Init.ExtFiltersNbr = 0;
        hfdcan1.Init.RxFifo0ElmtsNbr = 0;
        hfdcan1.Init.RxFifo0ElmtSize = FDCAN_DATA_BYTES_16;
        hfdcan1.Init.RxFifo1ElmtsNbr = 1;
        hfdcan1.Init.RxFifo1ElmtSize = FDCAN_DATA_BYTES_16;
        hfdcan1.Init.RxBuffersNbr = 1;
        hfdcan1.Init.RxBufferSize = FDCAN_DATA_BYTES_16;
        hfdcan1.Init.TxEventsNbr = 0;
        hfdcan1.Init.TxBuffersNbr = 0;
        hfdcan1.Init.TxFifoQueueElmtsNbr = 1;
        hfdcan1.Init.TxFifoQueueMode = FDCAN_TX_FIFO_OPERATION;
        hfdcan1.Init.TxElmtSize = FDCAN_DATA_BYTES_16;
     }
```

### CAN 메시지 송신 준비 및 ID 설정
**이 코드는 STM32에서 FDCAN 메시지를 송신하기 위해 사용되는 TxHeader 구조체의 필드들을 설정하는 부분이며 각 필드는 메시지 전송을 제어하고 정의하기 위해 사용**
  
```c
    TxHeader.Identifier = 0x11;
    TxHeader.IdType = FDCAN_STANDARD_ID;
    TxHeader.TxFrameType = FDCAN_DATA_FRAME;
    TxHeader.DataLength = FDCAN_DLC_BYTES_16;
    TxHeader.ErrorStateIndicator = FDCAN_ESI_ACTIVE;
    TxHeader.BitRateSwitch = FDCAN_BRS_OFF;
    TxHeader.FDFormat = FDCAN_FD_CAN;
    TxHeader.TxEventFifoControl = FDCAN_NO_TX_EVENTS;
    TxHeader.MessageMarker = 0x0;
```

### CAN 메시지 송신 준비 및 요청
**이 코드는 송신 메시지를 송신 FIFO에 추가하는 코드이다**
  
```c
    sprintf ((char *)TxData_Node1_To_Node3 ,"%d%d%d",Dist1,Dist2,Dist3);
    if(HAL_FDCAN_AddMessageToTxFifoQ(&hfdcan1, &TxHeader, TxData_Node1_To_Node3)!= HAL_OK)
    {
        Error_Handler();
    }
```

### CAN 수신 FIFO,BUFFER 설정
**이 코드는 STM32 HAL (Hardware Abstraction Layer) 라이브러리를 사용하여 FDCAN의 수신 콜백 함수를 정의하는 부분이며 각 콜백 함수는 FDCAN 수신 동작에서 특정 이벤트가 발생했을 때 호출되며, 수신된 데이터를 읽고 처리하는 역할을 한다. 이 프로젝트에선 두개의 FIFO와 한개의 BUFFER를 사용하였다**
```c
    void HAL_FDCAN_RxFifo0Callback(FDCAN_HandleTypeDef *hfdcan, uint32_t RxFifo0ITs)
    {
       if(FDCAN1 == hfdcan->Instance)
       {
	         if((RxFifo0ITs & FDCAN_IT_RX_FIFO0_NEW_MESSAGE) != RESET)
	         {
		        if (HAL_FDCAN_GetRxMessage(hfdcan, FDCAN_RX_FIFO0, &RxHeader, RxData_From_Node3) != HAL_OK)
		        {
		           Error_Handler();
		        }
	         }
       }
    }

    void HAL_FDCAN_RxFifo1Callback(FDCAN_HandleTypeDef *hfdcan, uint32_t RxFifo1ITs)
    {
       if(FDCAN1 == hfdcan->Instance)
       {
	         if((RxFifo1ITs & FDCAN_IT_RX_FIFO1_NEW_MESSAGE) != RESET)
	         {
	     	      if (HAL_FDCAN_GetRxMessage(hfdcan, FDCAN_RX_FIFO1, &RxHeader, RxData_From_Node1) != HAL_OK)
		           {
		              Error_Handler();
	           	}

	        }
      }
   }

   void HAL_FDCAN_RxBufferNewMessageCallback(FDCAN_HandleTypeDef *hfdcan)
   {
     if (FDCAN1 == hfdcan->Instance)
     {
        if (HAL_FDCAN_GetRxMessage(hfdcan, FDCAN_RX_BUFFER0, &RxHeader, RxData_From_Node4) != HAL_OK)
        {
            Error_Handler();
        }
    }
   }

```

### CAN 메시지 수신 대기 및 ID 필터링
**이 코드는 수신받고자하는 ECU의 ID를 필터링하는 코드이다**
```c
    sFilterConfig.IdType = FDCAN_STANDARD_ID; 
    sFilterConfig.FilterIndex = 0;  
    sFilterConfig.FilterType = FDCAN_FILTER_MASK; 
    sFilterConfig.FilterConfig = FDCAN_FILTER_TO_RXFIFO1; 
    sFilterConfig.FilterID1 = 0x44; 
    sFilterConfig.FilterID2 = 0x7ff; 
    sFilterConfig.RxBufferIndex = 0; 
```
### CAN 메시지 수신 확인 후 데이터 사용
**STM32CubeIDE의 Live Expression을 이용하여 수신을 확인**

![라이다_rx_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/5b00c63d-5d7f-41ef-b20b-22c977ad0201)

### Raspberry Pi CAN-Data 수신
**이 코드는 python-can 라이브러리를 사용하여 CAN 메시지를 생성하고 송신하는 부분**
```c
        bus = can.Bus(interface='socketcan',
              channel='can0', 
              receive_own_messages=True)   //CAN 버스를 설정하고 초기화. interface는 사용할 인터페이스를 지정하며, channel은 사용할 캔 채널을 지정한. 
	message = can.Message(arbitration_id=0x44, is_extended_id=False,data=[0x4C])  //보낼 CAN 메시지를 생성 arbitration_id는 메시지의 식별자(ID)를 나타낸다. is_extended_id는 식별자가 확장된 ID 형식인지 여부를 나타낸다. data는 메시지의 데이터를 지정
                  bus.send(message, timeout=0.2)  //생성한 메시지를 CAN 버스를 통해 송신 timeout은 메시지 송신의 제한 시간을 나타낸다.

```


**STM32 - Raspberry Pi WireShark를 이용하여 송수신을 확인**
 
![can_test_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/dafc5989-2f69-422b-8a81-247e9ec70b2f)


[본문으로 돌아가기](https://github.com/qkcvb110/Portfolio)
