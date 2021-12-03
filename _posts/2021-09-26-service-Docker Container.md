---
layout: post
title:  Docker Container
categories : Service

---

<H2> 인식모델 컨테이너화  </H2>


<h3>1️⃣ Docker </h3>
1. Docker Image Build  
```
$ docker build --tag eye-remocon/pose_detection:node .
```

2. Docker Image Run  
```
$ docker run -d -p 3500:3333 eye-remocon/pose_detection:node
```

3. 실행 결과 (실행된 로그 보는 명령)
1) 실행중인 컨테이너 ID 확인  
```
$ docker ps -all
```

2) 로그 확인  
```
$ docker logs --follow [컨테이너 ID]
```

3) 결과  
![image](https://user-images.githubusercontent.com/54658745/144575364-5742e80b-02ac-4695-bb9d-a5e22b3efbb5.png)
