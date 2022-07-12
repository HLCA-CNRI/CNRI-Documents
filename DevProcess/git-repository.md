# git-repository

## Structure

- upstream: HLCA-CNRI
- origin: 각자의 원격 저장소
- local: 각자의 로컬 저장소

## Flow

- 공통
  - git-flow 방식으로 작업 진행([main, develop] & [feature, release, hotfix, ...])
  - [feature, release, hotfix, ...] 내에서 sub-branch를 만든 경우, 작업 마무리 시 rebase 하여 history 정리하기
    - git pull --rebase 등 활용
- local -> origin
  - 각자 자유롭게 commit, push 가능
  - commit message convention 안 지켜도 됨
  - !! upstream으로 Pull Request 할 경우 commit history 정리 필요
    - commit message convention
    - git rebase -i 활용(squash)
- origin -> upstream
  - commit message convention 준수
  - upstream으로 바로 push하지 말고 Pull Request 하기
    - local - push -> origin
    - origin - Pull Request -> upstream
  - PR merge를 위해서는 1명 이상(차후 변경 가능)의 리뷰 필요
