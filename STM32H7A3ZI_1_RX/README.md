## 1. CAN,CAN-FD Communication
> - 프로젝트에 사용된 주 통신으로 CAN(Controller Area Network)통신을 사용
> - STM32 간은 CAN-FD, Raspberry Pi와 STM 간의 통신은 CAN을 사용하여 통신
 
<br/>

### CAN, CAN-FD 사용한 이유
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

**연결 방법**

![image](https://github.com/qkcvb110/Portfolio/assets/121782690/130d3b31-17a0-4d8d-b27c-d326193fdab8)

<br/>

### Flow Chart
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/5089d843-0405-4557-9870-9f5bc59a7c70" width="600" height="500"/>





> STM32 - Raspberry Pi CAN Communication Test
> 
> ![can_test_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/dafc5989-2f69-422b-8a81-247e9ec70b2f)
