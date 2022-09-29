# git-repository

git-flow와 함께 branch 관리를 위한 목적으로 활용

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
  - !! upstream으로 Pull Request 하기 전에 local & origin에서 commit history 정리 필요
    - commit message convention(맨 앞에 gitmoji 붙이기 등)
    - git rebase -i 활용(squash)
      - 동일하거나 비슷한 목적의 commit이 여러 개인 경우(이에 따라 여러 개의 commit message가 비슷한 경우) 합치기
      - ex. bugfix 1, bugfix 2, bugfix 3 -> bugfix
- origin -> upstream
  - commit message convention 준수
  - 관련 구성원 간 합의가 있지 않은 한 upstream으로 바로 push하지 말고 Pull Request 하기
    - local - push -> origin
    - origin - Pull Request -> upstream
  - PR merge를 위해서는 1명 이상(차후 변경 가능)의 리뷰 필요

## branch Rules

- git-flow:
  - feature:
    - 기본적으로 upstream까지 올리지 않음
  - release: upstream까지 올림
  - hotfix: upstream까지 올림
- push & Pull Request:
  - push
    - All: local -> origin
    - Only Owner: origin -> upstream
  - Pull Request
    - All: origin -> upstream

<참고 자료>

- [3.6 Git 브랜치 - Rebase 하기](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)
- ["git pull" vs "git pull --rebase"](https://jasonspace.tistory.com/11)
- [git pull --rebase 를 쓰자](https://jusths.tistory.com/60)
