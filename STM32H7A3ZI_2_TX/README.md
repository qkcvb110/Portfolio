# 5. Blind-Spot Collision-Avoidance Assist

## **📗 바로가기**

<b>

- [후측방 충돌방지 보조 기능](#blind-spot-collision-avoidance-assist)
- [CAN,CAN-FD를 사용한 이유](#사용-이유)
- [사용한 모듈 소개](#사용-모듈)
- [FlowChart](#flow-chart)
- [코드 설명](#코드-부분)
- [조종기](#조종기-구현)

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

## **구현 과정**
#### **후측면 초음파 센서 부착 모습**

> 
> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/1b345b4a-867b-4a61-aace-a672f441fa3e" width="300" height="400"/> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/40b30eea-344d-4474-af3e-52f0622c4878" width="300" height="400"/> <img src="https://github.com/qkcvb110/Portfolio/assets/121782690/f6496ba3-8ade-4fc1-994e-c4b32bd94941" width="300" height="400"/>

<br/>


