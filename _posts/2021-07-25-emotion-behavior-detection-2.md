---
layout: post
title: Emotion Detection Model-2 # 1
categories : Emotion-Behavior-Detection

---

<H2> AWS Rekognition 서비스와 결합  </H2>


<h3>1️⃣ 단순 AWS Rekognition 서비스 이용</h3>

- [가이드](https://docs.aws.amazon.com/ko_kr/rekognition/latest/dg/rekognition-dg.pdf) 참조하여 AWS Rekognition 서비스의 감정인식 테스트        
  - Issue 발생 => AWS InvalidS3ObjectException 문제    
  - Issue 해결 => AWS Rekognition 서비스 이용할 때 AWS S3 버킷에 이미지 파일 저장해야함    
- AWS Credentials 설정한 후 가이드 대로 따라하면 AWS Rekognition 서비스 간편하게 이용 가능      

<br>
<h3>2️⃣ 감정인식 기능별 구현 함수 상세화</h3>

* Main : 시작점  

* 서비스
  * 이미지 정보 수신 (Python flask 서버 코드 활용)
  * 감정인식 (Local 측면)
  * 감정인식 (AWS Rekognition 측면) 
  
* Local Emotion Detection
  * 크롭된 안면 이미지 파일 변환 (그레이화, 크기)
  * 미리 학습된 감정인식 모델 불러와서 해당 이미지에 인식된 감정인식 및 결과 반환  

* AWS Rekognition
  * 크롭된 안면 이미지 파일의 정보를 담은 요청구문을 통해 AWS Rekognition API 요청  
  * 응답된 JSON 파일의 Emotions 정보를 반환          

> Local Emotion detection model과 AWS Rekognition 서비스를 나눠서 실행  
> main에서 제일 먼저 Local Emotion detection model에서 감정인식된 결과값을 받기  
> 감정인식되지 않으면 "None"값을 반환하여 none값이면 AWS Rekognition 서비스 이용해서 감정인식 수행하도록 구현  

   
<br>

<h3>3️⃣ Flask 이용하여 emotion detection API 구현</h3>

1. 개발환경 구축
- 필요 패키지 설치(Linux & Mac)
  - Pytorch [공식 홈페이지](https://pytorch.org/get-started/locally/)에서 조건에 해당하는 pytorch 관련 라이브러리 설치
  - 
    ```
    $ pip install flask
    $ pip install boto3
    ```
- AWS 설정
  - [공식문서](https://docs.aws.amazon.com/ko_kr/rekognition/latest/dg/rekognition-dg.pdf) P.13 참고하여 access key, password 설정
  - 지역코드는 ap-northeast-2 로 입력



2. 소스코드

- main.py
  - flask 서버가 실행되는 메인 소스코드  
  - 클라이언트로부터 이미지파일을 POST 요청을 받아서, 해당 이미지의 BASE64코드를 이미지로 변환 수행  
  - 처음으로 HOME Emotion Detection을 수행하기 위해 detection() 함수 호출  
  - HOME Emotion Detection 수행 시, 감정인식이 되지 않는다면 아무값도 반환되지 않는다면, AWS Emotion Detection 에서 aws_main() 호출  
  - 반환되는 감정을 담은 json 파일을 클라이언트에게 다시 반환.   
  
```
import base64
import io
import numpy
import aws_emotion_detection
import home_emotion_detection
from PIL import Image
from flask import Flask, request, jsonify


app = Flask(__name__)
            
@app.route('/main', methods=['POST'])
def main():
    payload = request.form.to_dict(flat=False)
    im_b64 = payload['image'][0]
    im_binary = base64.b64decode(im_b64)
    buf = io.BytesIO(im_binary)
    face_img = Image.open(buf).convert('RGB')
    open_cv_face_image = numpy.array(face_img)

    detection_result = home_emotion_detection.detection(open_cv_face_image)
    if detection_result == None:
        detection_result = aws_emotion_detection.aws_main(im_binary)

    return jsonify({"emotion":detection_result})

if __name__ == "__main__":
    app.run(debug=True, host='0.0.0.0', port=9900)

```

<br>
<br>  


- home_emotion_detection.py
  - Home Emotion Detection을 위해 존재하는 소스코드입니다.
  - 미리 학습된 ResNet9 감정인식모델과 얼굴을 인식하는 모델을 불러옴
  - 사진의 얼굴을 인식하여 크롭하고, 감정인식모델에 적용될 수 있도록 이미지 변환 시키고 만약 얼굴이 인식되지 않았다면 No Face 반환
  - 학습된 모델을 forward 시켜 7가지 감정의 분류를 할 수 있도록 구현
  - 감정이 7가지로 분류된다면 해당 Label를 반환, 감정분류가 안된다면 공백 반환

   
```
import cv2
import numpy as np
import torch
import torch.nn as nn
import torch.nn.functional as F
import torchvision.transforms as tt

def conv_block(in_channels, out_channels, pool=False):
    layers = [nn.Conv2d(in_channels, out_channels, kernel_size=3, padding=1),
              nn.BatchNorm2d(out_channels),
              nn.ELU(inplace=True)]
    if pool: layers.append(nn.MaxPool2d(2))
    return nn.Sequential(*layers)

class ResNet(nn.Module):
    def __init__(self, in_channels, num_classes):
        super().__init__()

        self.conv1 = conv_block(in_channels, 128)
        self.conv2 = conv_block(128, 128, pool=True)
        self.res1 = nn.Sequential(conv_block(128, 128), conv_block(128, 128))
        self.drop1 = nn.Dropout(0.5)

        self.conv3 = conv_block(128, 256)
        self.conv4 = conv_block(256, 256, pool=True)
        self.res2 = nn.Sequential(conv_block(256, 256), conv_block(256, 256))
        self.drop2 = nn.Dropout(0.5)

        self.conv5 = conv_block(256, 512)
        self.conv6 = conv_block(512, 512, pool=True)
        self.res3 = nn.Sequential(conv_block(512, 512), conv_block(512, 512))
        self.drop3 = nn.Dropout(0.5)

        self.classifier = nn.Sequential(nn.MaxPool2d(6),
                                        nn.Flatten(),
                                        nn.Linear(512, num_classes))

    def forward(self, xb):
        out = self.conv1(xb)
        out = self.conv2(out)
        out = self.res1(out) + out
        out = self.drop1(out)

        out = self.conv3(out)
        out = self.conv4(out)
        out = self.res2(out) + out
        out = self.drop2(out)

        out = self.conv5(out)
        out = self.conv6(out)
        out = self.res3(out) + out
        out = self.drop3(out)

        out = self.classifier(out)
        return out

def detection(face_picture):
    face_classifier = cv2.CascadeClassifier('./models/haarcascade_frontalface_default.xml')
    model_state = torch.load('./models/emotion_detection_model_state.pth')
    class_labels = ['ANGRY', 'DISGUS', 'FEAR', 'HAPPY', 'NEUTRAL', 'SAD', 'SURPRISE']
    model = ResNet(1, len(class_labels))
    model.load_state_dict(model_state)

    gray = cv2.cvtColor(np.ascontiguousarray(face_picture), cv2.COLOR_BGR2GRAY)
    faces = face_classifier.detectMultiScale(gray, 1.3, 5)
    for (x, y, w, h) in faces:
        cv2.rectangle(face_picture, (x, y), (x + w, y + h), (255, 0, 0), 2)
        roi_gray = gray[y:y + h, x:x + w]
        roi_gray = cv2.resize(roi_gray, (48, 48), interpolation=cv2.INTER_AREA)

        if np.sum([roi_gray]) != 0:
            roi = tt.functional.to_pil_image(roi_gray)
            roi = tt.functional.to_grayscale(roi)
            roi = tt.ToTensor()(roi).unsqueeze(0)

            # make a prediction on the ROI
            tensor = model(roi)
            pred = torch.max(tensor, dim=1)[1].tolist()
            label = class_labels[pred[0]]
            return label

        else:
            return 'No Face'

```

<br>
<br>


- aws_emotion_detection.py
  - aws 감정인식 요청 시에, 이미지의 base64코드를 aws 로컬 이미지로 요청 
  - 기존 S3 클라우드 서비스에 이미지 저장하는 것을 변경함
  - 여러 FaceDetails 중에 가장 Confidence 높은 Emotions 특징값만 반환

 
```
#Copyright 2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.
#PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazonrekognition-developer-guide/blob/master/LICENSE-SAMPLECODE.)
import boto3


def detect_faces(photo):
    client = boto3.client('rekognition')
    response = client.detect_faces(Image={'Bytes': photo}, Attributes=['ALL'])
    for faceDetail in response['FaceDetails']:
        return faceDetail['Emotions'][0]

def aws_main(face_image):
    photo = face_image
    face_emotion = detect_faces(photo)
    return str(face_emotion['Type'])

```
<br>
<br>

<h3>4️⃣ 값진 코드 리뷰</h3>

- Flask 이용한 API 구현    
  - 개발 편의로 인한 host 주소 0.0.0.0 사용    
  - 자주 사용되는 포트(5000, 8080 등) 사용시 충돌이 날 수 있음으로 다른 포트 번호 사용 

  
```{.python}
if __name__ == "__main__":
    app.run(debug=True, host='0.0.0.0', port=9900)

```

<br>

- Local Emotion Detection에서 인식 결과 없을 때 ""로 반환이 아닌 "None"으로 반환


```{.python}
    detection_result = home_emotion_detection.detection(open_cv_face_image)
    if detection_result == None:
        detection_result = aws_emotion_detection.aws_main(im_binary)

```
 

