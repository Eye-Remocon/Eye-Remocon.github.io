---
layout: post
title: Modify DataSotrage md file
categories : LF-Edge-Contribution
---

<H2> DataStorage 가이드 문서 수정   </H2>


<h3>1️⃣ Modify DataStorage.md file based on the contents of docker-compose.yml </h3>  

![K-009](https://user-images.githubusercontent.com/54658745/144579198-5fe153bb-02d1-4f92-a1eb-fdfade0d41f1.png)

- Description : This PR is the contents of modified DataStorage.md file based on the creation of docker-compose.yml file.  
- Related Issue : [#296](https://github.com/lf-edge/edge-home-orchestration-go/pull/297)  
- [PR](https://github.com/lf-edge/edge-home-orchestration-go/pull/317)  
- Type of change : Documentation update  

<h3>2️⃣ 이슈 해결 위한 이해 과정 </h3>
- EdgeX의 version이 달라져서 Datastorage의 가이드 문서가 변경됨  
- 기존에 yml url로 가서 EdgeX 컨테이너를 build 했지만, deployments/datastorage 폴더에 docker파일로 edgeX의 컨테이너들 빌드 가능

<h3>3️⃣ 값진 코드 리뷰 </h3>
- 모든 사람들이 가이드 문서를 참조하기 때문에 or 이나 as 와 같은 접속사도 모두 다 옳바르게 표현해야함 
