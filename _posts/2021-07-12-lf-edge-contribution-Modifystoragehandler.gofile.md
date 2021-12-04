---
layout: post
title: Modify storagehandler.gofile
categories : LF-Edge-Contribution
---

<H2> Handler logger 수정 </H2>


<h3>1️⃣ Replace handler.logger in storagehandler.go file with logmgr </h3>

![K-010](https://user-images.githubusercontent.com/54658745/144579898-0175b7a0-e2fb-4a10-8bf0-eed20b8980d9.png)
- Description : Since DataStorage uses two types of logger, storagehandler.go file is modified.
  so, the handler.logger part of the storagehandler.go file was replaced wtih logmgr.
- Type of change : Code cleanup/refactoring
- [PR](https://github.com/lf-edge/edge-home-orchestration-go/pull/329)
- Related Issue : [#173](https://github.com/lf-edge/edge-home-orchestration-go/issues/173)


<br>

<h3>2️⃣ 컨트리뷰션을 위한 노력 </h3>        
- 이슈 1: 테스트 시 build failed 오류    
- ![image](https://user-images.githubusercontent.com/54658745/144581018-e1f624af-20ee-46fb-a87e-4bf61dc32ad2.png)

- 해결책
  - 원인 : 중복으로 선언되어 에러 발생, 다른 [package storagedriver](https://github.com/Eye-Remocon/edge-home-orchestration-go/blob/34ad844318457cb2cbae5d359cfa1d6ec0998411/internal/controller/storagemgr/storagedriver/storagedriver.go#L22)에서 이미 선언됨 
  - 해결 방법 : logmgr import 부분을 제거하면 log redeclared as imported package name 에러 해결 가능  


<h3>3️⃣ 값진 코드 리뷰 </h3>  
- 프로젝트 내 테스트 위해서 이미 테스트하는 모듈 존재  
- 테스트 파일을 활용해 테스트 후, 결과를 PR에 반영  
  - edge-home-orchestration-go 폴더    
    - $ go test -v ./internal/controller/storagemgr/storagedriver/  

  - edge-home-orchestration-go/internal/controller/storagemgr/storagedriver 폴더   
    - $ go test -v .  
