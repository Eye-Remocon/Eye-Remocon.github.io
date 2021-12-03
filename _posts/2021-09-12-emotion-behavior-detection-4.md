---
layout: post
title: Emotion Detection Model-4
categories : Emotion-Behavior-Detection

---

<H2> Pose Detection 성능 향상  </H2>


<h3>1️⃣ Pose Detection 응답 시간 단축</h3>

1. init() 함수를 통해서 
- 미리 학습된 모델을 불러온 후 사진 요청이 들어오면 pose detection 값을 반환하도록 구현
- 학습된 모델을 불러오는데 대략 3초정도 시간이 소요됐고, 3초를 단축

2. init() 함수 내에 canvas 설정 및 불러오는 부분을 미리 동작하도록 구현
- 0.5초의 시간도 단축할 수 있었습니다.
![image](https://user-images.githubusercontent.com/54658745/144572027-c9d00ed1-65bc-44b5-97de-494077b4b977.png)


3. 실제 img.onload 부분에서 해당 이미지가 pose detection에 적용될 수 없어서 pose predict value가 항상 동일한 값이 나오는 오류 발생
- [GitHub 주소](https://github.com/googlecreativelab/teachablemachine-community/issues/241)  
- 공식 teachablemachine-community GitHub에서 해당 이슈에 대해서 찾아보고 문제를 해결하여 정확한 pose detection 값을 반환 받도록 구현



<h3>2️⃣ 가만히 있는 Default pose 추가해 모델 재학습 </h3>

- 모든 이미지를 기존에 세 가지 행동(쓰러짐, 식사, 박수)로 인식하려는 경향이 있어 아무것도 안 하는 자세(stand)를 추가하여 다시 학습  

# 테스트 결과 
1. stand  
![image](https://user-images.githubusercontent.com/54658745/144572745-36064d36-0a8f-4d66-8c62-b5e2aa3dc81c.png)
2. emergency  
![image](https://user-images.githubusercontent.com/54658745/144572767-07b7fff2-16a7-462b-b14b-dec1d682e6be.png)
3. eat  
![image](https://user-images.githubusercontent.com/54658745/144572789-b8cf7a8c-7c50-4343-a2d1-4ddb28b0d587.png)
4. clap  
![image](https://user-images.githubusercontent.com/54658745/144572800-8a8d22ca-873d-41b1-ae70-a783baa07a0d.png)
