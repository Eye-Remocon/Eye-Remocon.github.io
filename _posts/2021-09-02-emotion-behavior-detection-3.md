---
layout: post
title: Emotion Detection Model-3
categories : Emotion-Behavior-Detection

---

<H2> Tensorflow 활용 행동인식 모델 학습   </H2>


<h3>1️⃣ Teachable Machine의 PoseNetModel 이용</h3>

- [링크](https://tfhub.dev/tensorflow/tfjs-model/posenet/resnet50/quantized/2/1/default/1) 에서 3가지 행동(쓰러짐, 식사, 박수)에 대해 인식할 수 있도록 학습      
- [링크](https://aihub.or.kr/aidata/138) AI Hub에서 학습 시 사람동작 영상 데이터 활용 
![K-002](https://user-images.githubusercontent.com/54658745/144569606-39ba9929-eaa7-471d-8c17-a7b60d426f50.png)
![K-003](https://user-images.githubusercontent.com/54658745/144569614-0a9794d7-0c5c-42f9-8e40-b4a72adda165.png)
![K-004](https://user-images.githubusercontent.com/54658745/144569621-f10d3a31-8d94-45d4-89c9-59880f857b29.png)


<br>
<h3>2️⃣ 이슈 발생</h3>

- ![K-005](https://user-images.githubusercontent.com/54658745/144570228-30c4e5e4-74dd-4bb6-ac10-67b6552e7bfe.png)
- Teachable Machine으로 학습한 모델을 API하기 위해 다른 코드 구현 필요
- 솔루션
  - 모델 불러서 인식 결과얻는 javascript를 node.js 사용해서 API화 하기 

<h3>3️⃣ node.js 활용해서 Pose detection API 구현</h3>

1. 개발환경 구축
- 개발 환경 세팅
  - node.js 설치 [참고](https://heropy.blog/2018/02/17/node-js-install/)  
  - 패키지 설치
  1) cd 해당프로젝트파일
  2) npm init -y
  3)
    ```
    $ npm i @teachablemachine/pose@0.8.6 --save
    $ npm i @tensorflow/tfjs@1.3.1 --save
    $ npm i canvas --save
    $ npm i express --save
    ```

2. python client code
- python requests 모듈 설치
```
$ pip install requests
```
- python code  

```
import requests
import base64
import time


start = time.time()
with open('이미지파일', 'rb') as f:
    im_b64 = base64.b64encode(f.read()).decode('utf8')

payload = {'img_base64':im_b64}
headers = {}
url = 'http://localhost:3000/pose_detection'

r = requests.post(url, json=payload, headers=headers)

if r.ok:
    pose = r.json()
    print(pose)
    print("time :", time.time() - start)
```

- 해당 이미지 파일을 테스트하려는 파이썬 프로젝트 파일경로에 넣어주시고 위에 코드를 알맞게 바꿔주시길 바랍니다.
- main.js 실행시킨 후 python 코드 실행시키면 다음과 같은 predict 된 json 파일을 얻으실 수 있습니다.
![image](https://user-images.githubusercontent.com/54658745/144573021-a15e680b-8bdd-4ae6-a412-ea3025ef8ab4.png)



4. main.js 코드 설명  
1) predict function
- teachablemacine에서 미리 학습된 pose 모델을 구글 drive에서 load
- client로부터 받은 img에 대한 base64코드를 buffer로 변환 
- 변환된 buffer를 canvas 모듈을 사용하여 Image를 형성
- 해당 Canvas Image로 부터 posenet 패키지를 사용하여 17개의 skeleton 추출하여 posenetOutput 얻기
- posenetOutput으로 부터 estimatePose하여 3가지 행동(응급, 식사, 박수)에 각각 확률값 json형태로 반환
2) express api 실행 부분  
- client post request를 받아 pose 예측값들을 response


  

