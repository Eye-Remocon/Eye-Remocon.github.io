---
layout: post
title: Emotion Detection Model-1 # 1
categories : Emotion-Behavior-Detection

---

<H2>1️⃣ CNN ResNet9 감정인식 모델 구축 </H2>

# [출처](https://medium.com/swlh/emotion-detection-using-pytorch-4f6fbfd14b2e)

1. 데이터 준비
- kaggle, [Emotion DataSet](https://www.kaggle.com/dataset/de270025c781ba47a3a6d774a0d670452bfb4dc9d2d6b13740cdb0c17aa7bf2b)
- Emotion : Angry, Disgust, Fear, Happy, Neutral, Sad, Surprise
![K-008](https://user-images.githubusercontent.com/54658745/124116816-5927ad00-daaa-11eb-9278-9a8b18cc7248.png)


2. 데이터 변환 작업
- torch에서 dataset모듈로 데이터 읽어 오고, transforms 모듈로 불러온 이미지를 필요에 따라 변환
- Grayscale(이미지 회색 변환), 50%의 확률(랜덤)으로 이미지 가로로 돌리기
- RandomRotation으로 이미지 30도(왼쪽, 오른쪽 랜덤)으로 돌리기
- 위 랜덤으로 이미지 변환을 통해 과적합을 피함, 과적합(overfitting)이란 학습 데이터셋 안에서 일정 수준 이상의 예측 정확도를 보이지만, 새로운 데이터에 적용하면 잘 맞지 않는 것을 의미
- ToTensor => 이미지 데이터를 파이토치 텐서로 변환함, Pytorch layers는 오직 tensor 데이터 단위로 동작

![K-010](https://user-images.githubusercontent.com/54658745/124117482-2c27ca00-daab-11eb-803b-835a8ded4e72.png)


3. Loader data to GPU
- cuda, cudnn 라이브러리를 이용해서 GPU를 이용해서 모델 학습

4. ResNet9 Model Base Lines
```{.python}
def conv_block(in_channels, out_channels, pool=False):
    layers = [nn.Conv2d(in_channels, out_channels, kernel_size=3, padding=1), 
              nn.BatchNorm2d(out_channels), 
              nn.ELU(inplace=True)]
    if pool: layers.append(nn.MaxPool2d(2))
    return nn.Sequential(*layers)

class ResNet(ImageClassificationBase):
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
```


5. Hyperparameters
```{.python}
epochs = 140
max_lr = 0.008
grad_clip = 0.1
weight_decay = 1e-4
opt_func = torch.optim.Adam
```

6. 학습결과

![K-005](https://user-images.githubusercontent.com/54658745/124118750-a3119280-daac-11eb-9aca-7512d8378b7d.png)

<H2> 2️⃣ CNN ResNet9 감정인식 모델 테스트 </H2>

- face 폴더에 감정을 인식할 사진들 저장함  
![image](https://user-images.githubusercontent.com/54658745/124119467-86c22580-daad-11eb-9e05-7ef594b224fd.png)

- 테스트 결과 4번사람 제외하고 정확하게 감정인식된 결과 확인 가능  
![image](https://user-images.githubusercontent.com/54658745/124119540-9d687c80-daad-11eb-9926-83e10d1e1e58.png)  


<H2> 3️⃣ 결론 </H2>
- 라이센스 문제와, 정확도 54% ~ 55% 정도로 다소 낮은 문제로 정확도를 높일 수 있는 방법 찾아 모델 발전시키로 결정. 또한 직접 학습하는 방법 이외에, 3개 회사 [API](https://www.github.com/Eye-Remocon/Emotion_detection/blob/main/README.md) 사용하기로 결정
