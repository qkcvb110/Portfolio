# 3. Lane Keeping Assist

## **📗 바로가기**

<b>

- [Foward Collision-Avoidance Assist](#foward-collision-avoidance-assist)
- [사용 센서](#tf-luna-lidar-sensor)
- [FlowChart](#flow-chart)
- [코드 설명](#코드-부분)
- [구현 과정](#구현-과정)

## **Lane Keeping Assist**
###  주행 중 차량이 차로를 벗어나지 않도록 보조는 시스템이다. 일정 속도 이상에서 운전자가 방향지시등을 켜지 않고 차로를 이탈하려고 하면 운전자에게 경고하며, 설정에 따라 스티어링 휠을 직접 제어해 차로를 벗어나지 않도록 보조
![IGDteMXJiS1D7rvKvedIwjY2zTXBTPd_a3jmhKNYNfFw4ZnjVbamcXz_53uwGT70ceO-w6NCOGojYrz0eP5gaA (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/9d11cc02-4075-4827-809f-d05ffaa5de66)


> - ADAS의 기능 중 차선보조기능을 선택하여 구현
> - 보드는 Raspberry Pi 4B 8GB 모델을 선택하여 실시간 데이터를 STM32와 CAN 통신으로 전송
> - OpenCV를 통하여 구현

### Flow Chart
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/8d1c1a76-bfd3-4fdd-b29f-eab33ac356b1" width="400" height="500"/>

###  주요 기능 
------------
* 차선 인식
> **흑백화**
> **흑백화를 하는 이유**
> - 흑백 이미지로 변환하면 컬러 정보가 제거되고 간단한 밝기 정보만을 다룰 수 있게 되며 알고리즘의 처리 단순화에 도움
> - RGB 처리는 연산이 더욱 복잡하므로 흑백화를 하면 연산이 수월해
```c
gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)
```
> **모서리 검출**
> - 모서리를 검출하여 선의 경계들을 인식
> - 이때까진 차선만을 검출하기엔 부족
```c
can = cv2.Canny(gray, 50, 200, None, 3)
```
> **관심구역 설정**
> - 설치한 카메라의 각도를 보고 관심구역을 설정해 차선이 아닌 다른것은 인식을 하지 않도록 설정
```c
 height, width = can.shape[:2]
 top_left = (width // 12, height)
 top_right = (width * 11 // 12, height)
 bottom_right = (width * 11 // 12, height * 2 // 3)
 bottom_left = (width // 12, height * 2 // 3)
 roi_vertices = [top_left, top_right, bottom_right, bottom_left]
```
> **직선 검출**
```c
line_arr = cv2.HoughLinesP(masked_image, 1, np.pi / 180, 20, minLineLength=10, maxLineGap=10)
```
> **원본에 합성**
```c
mimg = cv2.addWeighted(src, 1, ccan, 1, 0)
```

<br>

> **방향제어**
> 
> **arctan2 함수를 사용하여 각도를 계산하여 각도에 따른 진행 방향을 결정**
```c
 line_arr2[i] = np.append(line_arr[i], np.array((np.arctan2(l[1] - l[3], l[0] - l[2]) * 180) / np.pi))
```

> **여러 테스트 진행 후 각도에 따른 진행 방향 정확도 증가**
> 
> **arctan2 함수를 사용하여 각도를 계산하여 각도에 따른 진행 방향을 결정**
> - 진행해본 여러 테스트를 기반으로 오류값을 찾아 그 값의 범위를 재설정하여 오차를 줄여 안정성을 증가
> - 차선을 두줄을 인식하는 경우와 차선이 한줄만 인식하는 경우의 범위를 모두 설정
```c
 if l == 0 or r == 0:
            if (105 <= l and l<= 112) or (105 <= r and r<= 112) :
                print('right')

            elif (112 < l and l<= 200) or (112 < r and r<= 200) :
                print('Hard right')

            elif (90 < abs(l) and abs(l)< 105) or (90 < abs(r) and abs(r)< 105) :
                print('go')

            elif ( l < -113 ) or ( r < -113) :
                print('Hard left')

            else :
                print('left')

        elif l == 0 and r == 0  :
            print('go')

        elif (90 < abs(l) and abs(l)< 105) and (90 < abs(r) and abs(r)< 105) :
            print('go')
 
        elif (105 <= l and l<= 112) and (95 <= r and r<= 112) :
            print('right')

        elif (112 < l and l<= 200) and (98 <= r and r<= 200) :
            print('Hard right')

        elif ( l < -113 ) and ( r < -113) :
            print('Hard left')

        else :
            print('left')

```
> **CAN-Data 송신**
> 
> **결정된 방향을 CAN-Data로 정의 후 송신**
> id=0x44 ID는 0x44로 설정 후 데이터 0x4C(L)을 송신
```c
 message = can.Message(arbitration_id=0x44, is_extended_id=False,data=[0x4C])
           bus.send(message, timeout=0.2)
```

<br/>

> #### 기능 구현 후 첫 테스트
> 
> ![차선인식테스트_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/75f805c7-3357-412b-9420-d1ffd46ef9eb)
> ![라즈베리 차선인식](https://github.com/qkcvb110/Portfolio/assets/121782690/c3da885e-9eda-4c7c-9015-912b6ff8a5f4)

<br/>

> #### 직진 차선 인식테스트
> #### 개선할점
> - 카메라의 위치
> - 인식은 조향장치의 약 30~50cm 앞을 하므로 이 부분을 개선
>
> ![직진_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/9f3dbd1c-e622-4752-bfe8-0087a32853cf)

<br/>

> #### 문제점 개선 후 최종 우회전과 좌회전
> - 카메라의 위치 조정과 조향장치의 앞을 인식하는 문제점을 개선하여 차선보조기능 마무리
>
> ![차선인식_우회전_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/8905004f-a68d-4041-b79e-c9f64a960ff0)
 ![차선인식_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/7245e01e-ee5e-436e-9c34-db893c3bc78b)

[본문으로 돌아가기](https://github.com/qkcvb110/Portfolio)
