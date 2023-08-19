# 3. Lane Keeping Assist
> - ADAS의 기능 중 차선보조기능을 선택하여 구현
> - 보드는 Raspberry Pi 4B 8GB 모델을 선택하여 실시간 데이터를 STM32와 CAN 통신으로 전송
> - OpenCV를 통하여 구현

### Flow Chart
<img src="https://github.com/qkcvb110/Portfolio/assets/121782690/8d1c1a76-bfd3-4fdd-b29f-eab33ac356b1" width="400" height="500"/>

###  주요 기능 
------------
* 차선 인식
> **흑백화**
```c
gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)
```
> **모서리 검출**
```c
can = cv2.Canny(gray, 50, 200, None, 3)
```
> **관심구역 설정**
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
```c
 message = can.Message(arbitration_id=0x44, is_extended_id=False,data=[0x4C])
           bus.send(message, timeout=0.2)
```

<br/>

> #### 기능 구현 후 첫 테스트
> 
> ![차선인식테스트_AdobeExpress (1)](https://github.com/qkcvb110/Portfolio/assets/121782690/75f805c7-3357-412b-9420-d1ffd46ef9eb)
> 
> #### 직진 차선 인식테스트
> #### 개선할점
> - 카메라의 위치
> - 인식은 조향장치의 약 30~50cm 앞을 하므로 이 부분을 개선
> 
> ![직진_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/9f3dbd1c-e622-4752-bfe8-0087a32853cf)
>
> #### 문제점 개선 후 최종 우회전과 좌회전
> - 카메라의 위치 조정과 조향장치의 앞을 인식하는 문제점을 개선하여 차선보조기능 마무리
>
> ![차선인식_우회전_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/8905004f-a68d-4041-b79e-c9f64a960ff0)
 ![차선인식_AdobeExpress](https://github.com/qkcvb110/Portfolio/assets/121782690/7245e01e-ee5e-436e-9c34-db893c3bc78b)

[본문으로 돌아가기](https://github.com/qkcvb110/Portfolio)
