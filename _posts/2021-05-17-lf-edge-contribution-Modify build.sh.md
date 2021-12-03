---
layout: post
title: Modify build.sh
categories : LF-Edge-Contribution
---

<H2> Project Build.sh 수정   </H2>


<h3>1️⃣ Modify build.sh and tools/create_fs.sh </h3>

![K-008](https://user-images.githubusercontent.com/54658745/144577008-d46d2dac-3906-45b2-87d5-7d5dfcc723db.png)
- Description : I modified the build.sh and tools/create_fs.sh files to create /var/edge-orchestration/user/orchestration_userID.txt.
- Type of change : Code cleanup/refactoring
- [PR](https://github.com/lf-edge/edge-home-orchestration-go/pull/303)


<br>
<h3>2️⃣ 값진 코드 리뷰 </h3>

- $ git commit 하실 때 --sign-off 넣기
  - 오픈소스 컨트리뷰션에 커밋에 Signed-off-by: [이름] [이메일] 식으로 반영 필요 
  - 참고 : [3e36d86](https://github.com/Eye-Remocon/edge-home-orchestration-go/commit/3e36d86ec3906872b84ba3cb17175b74c08b256a)
- TDD 개발 방식에 맞춰 다른 사람이 쉽게 테스트 할 수 있도록 'How Has This Been Tested?' 작성 필요
- 코드 내 쓸데없는 빈줄, 빈칸 추가는 코드의 가독성을 낮출 수 있으므로 주의
