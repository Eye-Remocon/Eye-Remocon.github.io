---
layout: post
title: Modify Storage.go file
categories : LF-Edge-Contribution
---

<H2> [DataStorage]Replace getDeviceID functions in storage.go file with helper.go  </H2>

<h3>1️⃣ Modify build.sh and tools/create_fs.sh </h3>

![K-011](https://user-images.githubusercontent.com/54658745/144582098-a723d9fe-b32c-4ae0-b42c-6611a54ff5ce.png)

- Description : We have to replace getDeviceID in storage.go file with GetDevice function.
So, I called GetDevice() in the helper.go file and changed it.
- Type of change : Code cleanup/refactoring
- [PR](https://github.com/lf-edge/edge-home-orchestration-go/pull/410)
- Related Issue : [#321](https://github.com/lf-edge/edge-home-orchestration-go/issues/321)


<br>
<h3>2️⃣ 컨트리뷰션을 위한 과정 </h3>

1. fmt workflow 오류 발생
- [링크](https://github.com/lf-edge/edge-home-orchestration-go/issues/321)
- 해결책 : go fmt 사용
  - $ go fmt ./internal/... 
    - fmt 사용하면 자동으로 위치, 띄어쓰기 포맷을 변경해줌
  - $ git diff 
    - 수정이 필요한 곳을 확인 가능
    
2. 내부 함수 getDeviceID()와 외부 함수 GetDeviceID()의 반환값이 같지 않음
- 해결책 : logmgr로 로그를 찍어 같은 반환값인지 확인
- logmgr 사용하기 위해서 [링크](https://github.com/lf-edge/edge-home-orchestration-go/blob/master/docs/datastorage.md#43-run-edge-orchestration) EdgeX Foundry 활용위해 컨테이너 빌드
  - 다른 이슈 : logmgr로 로그 확인시 반환값이 빈칸으로 아무런 값이 나타나지 않음
  ![image](https://user-images.githubusercontent.com/54658745/144582962-5da7dc77-5f45-435e-9b9b-5df5d8eec54e.png)  
  - 해결책 : deviceID가 DB에 저장되기 전에 init()이 호출되어 값이 없음. 따라서 StartStorage()에서 먼저 GetDeviceID()에서 호출하는 위치 조정 필요


