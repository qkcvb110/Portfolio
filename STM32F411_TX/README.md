# 2. RF Communication

## **📗 바로가기**

<b>

- [RF 통신](#rf-통신이란)
- [CAN,CAN-FD를 사용한 이유](#사용-이유)
- [사용한 모듈 소개](#사용-모듈)
- [FlowChart](#flow-chart)
- [코드 설명](#코드-부분)
- [조종기](#조종기-구현)

<br/>

## **RF 통신이란**
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

## **Flow Chart**
<img width="800" alt="스크린샷 2023-08-21 174313" src="https://github.com/qkcvb110/Portfolio/assets/121782690/328f66da-ebdd-457e-8aa0-7dfd48605139">

<br/>

# 코드 부분
## NRF 초기화 및 설정
#### 이 함수는 모듈의 다양한 레지스터를 설정하여 모듈을 초기화하고 기본 동작을 설정
> - 각각 레지스터 초기화, 자동 ACK(AckKnowledgment) 비활성화, 데이터 파이프(Data Pipe), 주소의 너비, RF설정 초기화, 전력 0dB, 데이터 속도를 2Mbps로 설정
```c
    void NRF24_Init (void)
    {
	// disable the chip before configuring the device
	CE_Disable();


	// reset everything
	nrf24_reset (0);

	nrf24_WriteReg(CONFIG, 0);  // will be configured later

	nrf24_WriteReg(EN_AA, 0);  // No Auto ACK

	nrf24_WriteReg (EN_RXADDR, 0);  // Not Enabling any data pipe right now

	nrf24_WriteReg (SETUP_AW, 0x03);  // 5 Bytes for the TX/RX address

	nrf24_WriteReg (SETUP_RETR, 0);   // No retransmission

	nrf24_WriteReg (RF_CH, 0);  // will be setup during Tx or RX

	nrf24_WriteReg (RF_SETUP, 0x0E);   // Power= 0db, data rate = 2Mbps

	// Enable the chip after configuring the device
	CE_Enable();

    }
```
<br/>

## NRF 송신모드 변경 및 주소값, 채널값 설정
#### 이 함수는 NRF24L01 무선 통신 모듈을 송신 모드로 설정하는 역할을 하며, 채널과 주소를 설정하여 데이터를 송신 준비
> - 무선 통신 모듈의 동작을 설정하기 전에 Chip Enable (CE) 핀을 비활성화하여 무선 송수신을 중지
> - 무선 채널을 설정 송수신 모드에서는 데이터를 주고받는 채널을 선택
> - 송신 주소와 레지스터에 주소를 설정
> - 레지스터 값을 읽어온 후 레지스터 값을 수정합니다. 비트 마스킹을 사용하여 PRIM_RX 비트는 0으로, PWR_UP 비트는 1로 설정
> - 설정이 완료된 후에 Chip Enable (CE) 핀을 활성화하여 모듈을 송신 모드로 전환

```c
void NRF24_TxMode (uint8_t *Address, uint8_t channel)
{
	// disable the chip before configuring the device
	CE_Disable();

	nrf24_WriteReg (RF_CH, channel);  // select the channel

	nrf24_WriteRegMulti(TX_ADDR, Address, 5);  // Write the TX address


	// power up the device
	uint8_t config = nrf24_ReadReg(CONFIG);
	config = config & (0xF2);    // write 0 in the PRIM_RX, and 1 in the PWR_UP, and all other bits are masked
	nrf24_WriteReg (CONFIG, config);

	// Enable the chip after configuring the device
	CE_Enable();
}

```

<br/>

## NRF 수신모드 변경 및 주소값, 채널값 설정
#### 이 함수는 NRF24L01 무선 통신 모듈을 수신 모드로 설정하는 역할을 하며, 채널과 주소 설정을 통해 데이터 수신을 준비
> - 무선 통신 모듈의 동작을 설정하기 전에 Chip Enable (CE) 핀을 비활성화하여 무선 송수신을 중지
> - 레지스터를 초기화하여 상태 비트를 초기화
> - 무선 채널을 설정 후 수신 모드에서는 데이터를 주고받는 채널을 선택
> - 레지스터의 값을 읽어온 후 레지스터의 2번째 비트를 1로 설정하여 Data Pipe 2를 활성화
> - 수정된 en_rxaddr 값을 다시 EN_RXADDR 레지스터에 적용
> - 레지스터를 사용하여 Pipe 2의 페이로드 크기를 32비트로 설정
> - 레지스터 값을 수정하여 PRIM_RX와 PWR_UP 비트를 설정
> - 설정이 완료된 후에 Chip Enable (CE) 핀을 활성화하여 모듈을 수신 모드로 전환

```c
void NRF24_RxMode (uint8_t *Address, uint8_t channel)
{
	// disable the chip before configuring the device
	CE_Disable();

	nrf24_reset (STATUS);

	nrf24_WriteReg (RF_CH, channel);  // select the channel

	// select data pipe 2
	uint8_t en_rxaddr = nrf24_ReadReg(EN_RXADDR);
	en_rxaddr = en_rxaddr | (1<<2);
	nrf24_WriteReg (EN_RXADDR, en_rxaddr);

	/* We must write the address for Data Pipe 1, if we want to use any pipe from 2 to 5
	 * The Address from DATA Pipe 2 to Data Pipe 5 differs only in the LSB
	 * Their 4 MSB Bytes will still be same as Data Pipe 1
	 *
	 * For Eg->
	 * Pipe 1 ADDR = 0xAABBCCDD11
	 * Pipe 2 ADDR = 0xAABBCCDD22
	 * Pipe 3 ADDR = 0xAABBCCDD33
	 *
	 */
	nrf24_WriteRegMulti(RX_ADDR_P1, Address, 5);  // Write the Pipe1 address
	nrf24_WriteReg(RX_ADDR_P2, 0xEE);  // Write the Pipe2 LSB address

	nrf24_WriteReg (RX_PW_P2, 32);   // 32 bit payload size for pipe 2


	// power up the device in Rx mode
	uint8_t config = nrf24_ReadReg(CONFIG);
	config = config | (1<<1) | (1<<0);
	nrf24_WriteReg (CONFIG, config);

	// Enable the chip after configuring the device
	CE_Enable();
}

```

<br/>

## 송신모드 FIFO 초기화 및 송신 과정
#### 이 함수는 NRF24L01 무선 통신 모듈을 사용하여 데이터를 송신하는 역할을 하며 먼저 데이터를 모듈에 적재하고 전송하며, 송신 상태를 확인하여 성공 여부를 반환
> - 무선 통신 모듈을 선택
> - 데이터를 전송하기 위한 페이로드 커맨드를 설정
> - 설정한 페이로드 커맨드를 SPI 통신을 통해 모듈에 전송
> - 실제 데이터를 모듈에 전송
> - 무선 통신 모듈의 선택을 해제
> - FIFO_STATUS 레지스터의 네 번째 비트가 1 (TX FIFO가 비어 있음)이고, 세 번째 비트가 0 (TX FIFO가 차있음)인지 확인
> - TX FIFO를 비우기 위해 FLUSH_TX 커맨드를 설정
> - 커맨드를 모듈에 전송
> - IFO_STATUS 레지스터를 초기화하여 상태를 재설정
> - 송신 성공을 나타내는 1을 반환
> - 송신 실패를 나타내는 0을 반환

```c
uint8_t NRF24_Transmit (uint8_t *data)
{
	uint8_t cmdtosend = 0;

	// select the device
	CS_Select();

	// payload command
	cmdtosend = W_TX_PAYLOAD;
	HAL_SPI_Transmit(NRF24_SPI, &cmdtosend, 1, 100);

	// send the payload
	HAL_SPI_Transmit(NRF24_SPI, data, 32, 1000);

	// Unselect the device
	CS_UnSelect();

	HAL_Delay(1);

	uint8_t fifostatus = nrf24_ReadReg(FIFO_STATUS);

	// check the fourth bit of FIFO_STATUS to know if the TX fifo is empty
	if ((fifostatus&(1<<4)) && (!(fifostatus&(1<<3))))
	{
		cmdtosend = FLUSH_TX;
		nrfsendCmd(cmdtosend);

		// reset FIFO_STATUS
		nrf24_reset (FIFO_STATUS);

		return 1;
	}

	return 0;
}
```

<br/>

## 수신모드 FIFO 초기화 및 수신 과정
#### 이 함수는 NRF24L01 무선 통신 모듈을 사용하여 데이터를 수신하는 역할을 하며 수신된 데이터를 버퍼에 저장하고, 수신 상태를 초기화하여 다음 데이터 수신을 준비
> - 무선 통신 모듈을 선택
> - 데이터를 수신하기 위한 페이로드 수신 커맨드를 설정
> - 설정한 페이로드 수신 커맨드를 SPI 통신을 통해 모듈에 전송
> - 데이터를 모듈에서 수신하여 data 버퍼에 저장(32바이트의 데이터를 수신하도록 설정)
> - 무선 통신 모듈의 선택을 해제
> - RX FIFO를 비우기 위해 FLUSH_RX 커맨드를 설정
> - 커맨드를 모듈에 전송

```c
void NRF24_Receive (uint8_t *data)
{
	uint8_t cmdtosend = 0;

	// select the device
	CS_Select();

	// payload command
	cmdtosend = R_RX_PAYLOAD;
	HAL_SPI_Transmit(NRF24_SPI, &cmdtosend, 1, 100);

	// Receive the payload
	HAL_SPI_Receive(NRF24_SPI, data, 32, 1000);

	// Unselect the device
	CS_UnSelect();

	HAL_Delay(1);

	cmdtosend = FLUSH_RX;
	nrfsendCmd(cmdtosend);
}
```

## **조종기 구현**
### 조종기로는 핸들과 엑셀레이터를 직접제작하여 가변저항의 ADC값을 RF로 통신
> - **조종기 전체 모습**
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/ffb32203-ef52-4cc4-9996-8100f3610fce.png" width="300" height="400"/>
>
> <br/>
> 
> - **각 핸들과 엑셀레이터에 가변저항 부착 모습**
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
