---
layout: post
title: HW-control Server
categories : HW-control
---

<H2> AWS Rekognition 서비스와 결합  </H2>


<h3>1️⃣ Flask 활용 라즈베리파이 LED 중개 서버</h3>

- arduino/led.ino  
    - 기존의 3컬러에서 프로젝트 내용에 맞춰서 7가지 감정에 도움을 주는 색에 맞춰 RGB색 변경할 수 있도록 하였습니다.  
- led/led.py  
    - def color_change(emotion, power): 조명의 전원(power=True)가 on인 상태에서만 해당 감정에 맞춰 색 변경에 대한 시리얼 통신을 하도록 구현하였습니다.  
    - def off(): LED 전원을 off 하도록 구현하였습니다.  
    - def on(): LED 전원을 on 하도록 구현하였습니다. 
- main.py  
    - def emotion_change(): 감정인식 후 LED 색 변화 요청이 올 때 서버에서 동작하는 것을 구현하였습니다.      
                            LED가 on(power=True)인 상태에서만 LED 색이 변경됩니다.  
    - def pose_change(): 행동인식 후 HW 컨트롤 요청이 올 때 서버에서 동작하는 것을 구현하였습니다.  
                           박수동작을 통해 LED 전원을 ON/OFF 합니다(POWER 값도 변경)
    - def emergency(): 응급상황 인식 후 led가 깜박일 수 있는 기능을 구현하였습니다.

- 처음 LED 연결시 LED 전원(Power = True) 즉, 켜져 있는 상태입니다.  
- 박수동작 인식했을 때 LED전원(power=True) on인 상태라면 power=False로 변경하고 off()함수를 통해 LED 전원을 끄도록 하였습니다.  

<br>
<h3>2️⃣ 감정인식의 7가지 감정에 도움줄 수 있는 색</h3>

![K-006](https://user-images.githubusercontent.com/54658745/144573697-b1a83078-5ad5-49d7-a019-b0078bcbba63.png)  
![K-007](https://user-images.githubusercontent.com/54658745/144573713-fba12c97-6dad-4383-af66-2325849c6c70.png)

