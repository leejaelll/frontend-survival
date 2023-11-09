---
description: Git 명령어 모아놓기
---

# Git

### ✅ branch clone 이후 branch 변경하는 방법

- Gitlab에서 repository를 clone 후, git branch를 했을 때 모든 브랜치가 확인되지 않는 경우가 있다.
- 이 때 `git branch -a`로 리모트에 브랜치가 있는지 먼저 확인한다.
- `git checkout -t origin/{branch_name}` 👉🏻 원격 저장소의 해당 브랜치를 로컬에 생성한 뒤, 해당 브랜치로 이동한다.

---
