# 4. Foward Collision-Avoidance Assist

## **📗 바로가기**

<b>

- [RF 통신](#rf-통신이란)
- [CAN,CAN-FD를 사용한 이유](#사용-이유)
- [사용한 모듈 소개](#사용-모듈)
- [FlowChart](#flow-chart)
- [코드 설명](#코드-부분)
- [조종기](#조종기-구현)

<br/>

## **Foward Collision-Avoidance Assist(충돌 방지 보조 시스템)**
### 전방충돌 경고 기능에서 한 단계 더 나아가, 단순 경고뿐만 아니라 차량의 제동까지 직접 보조하는 기능
![HyAnc97QDwSBtT93gcakbvsx2L3MpiN771YD0ZaLfraXffZyGWCj9iBpdHxZ3dYl_uFkzlHz1D9FkEBTBc9npQ_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/7e52c0e3-1b79-4b60-9137-22bcc24fb3dc)

<br/>

## **TF-Luna Lidar Sensor**
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/5d20d854-fddf-4648-a433-8d3a10a4e279" width="250" height="200"/>
 <img width="271" alt="스크린샷 2023-08-21 185837" src="https://github.com/qkcvb110/Portfolio/assets/121782690/b81c782e-b9ac-4557-9e8d-05b6f524c376">
 
> - 차량의 전방의 장애물과의 거리를 측정하기 위한 센서로 사용

## **사용 이유**
> - LiDAR 기술: TF-Luna는 Time-of-Flight (ToF) LiDAR 기술을 사용하여 거리 측정을 수행하 이 기술은 빛의 시간을 측정하여 거리를 계산하므로 정확한 거리 측정이 가능
> - 높은 정확도: ToF LiDAR 기술은 초음파 센서보다 높은 정확도를 가지며, 정확한 거리 및 위치 측정이 필요한 응용 프로그램에 적합
> - 빠른 응답 속도: ToF LiDAR는 빛의 속도를 기반으로 하기 때문에 빠른 응답 속도를 가지며 이로 인해 빠르게 변하는 환경에서도 정확한 측정이 가능
> - 장거리 측정: TF-Luna 모델은 장거리 측정이 가능하여 초음파 센서보다 더 넓은 범위에서 사용(0.2M ~ 8M 측정가능)

<br/>

## **Flow Chart**


<br/>

# 코드 부분




> #### LIDAR CAN-FD 통신
>
> ![라이다_rx_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/be592a73-d340-4922-913a-26ee13bdb912) ![라이다_tx_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/ee5b5969-6770-4355-a4d8-546fede506e4)
>  
> #### LIDAR 테스트 영상
>
> ![라이다_테스트_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/600dd67f-8e80-4219-93ed-bef1ae5e63d0)
