# git-flow

## Summary

- main: 최종, 완전한 상태의 배포판
- develop: 기능 추가, 성능 개선, 버그 픽스 등 전체적인 개발 과정에서 사용되는 branch.
  - 주의사항: develop branch 내부적으로 commit하여 변경할 수 없고, feature 또는 hotfix에 의해서만 변경됨.
- feature: 기능 추가, 버그 픽스를 위한 branch. Github Issues 기준으로 수행.
- hotfix: 완전한 줄 알았던 main에서 버그가 발생한 경우 급하게 고치기 위한 branch
- release: 일정 범위 내의 기능들을 종합하여 배포하기 위한 branch.
  - 이때 배포를 위해 최종적인 버그 수정 등을 진행함

## How to Use

1. git 설치
2. git-flow 설치

```
// Windows: git bash 설치 시 같이 설치됨

// Ubuntu
apt-get install git-flow

// Mac
brew install git-flow-avh
```

3. 초기화

```
git flow init
```

4. 새 기능(feature) 개발 작업 시작하기 & 기능 개발 작업 완료하기 & 기능 개발 공유하기

```
// 기능 개발 시작하기
// MY_FEATURE에는 기능의 이름 혹은 Github Issues 번호를 넣음
// (CNRI는 Issues 번호를 넣음)
git flow feature start MY_FEATURE

// 기능 개발 완료하기
git flow feature finish MY_FEATURE

// 원격 repo에 기능 업로드하기
// 여러 명이 하나의 기능을 개발할 경우 -> 대표자 한 명의 origin을 다른 사람들이 각자 remote add 하여 연결하기
// 이렇게 하면 굳이 upstream까지 올려서 서로 공유할 필요 없이 대표자의 origin을 기준으로 작업하면 됨
git flow feature publish MY_FEATURE

// 원격 repo에 있는 기능 받아오기
// 바로 위의 publish 쪽에서 설명한 것처럼 여럿이 작업하는 경우,
// REMOTE_NAME에는 대표자의 origin을 remote add할 때 설정한 값을 입력하면 됨
git flow feature pull REMOTE_NAME MY_FEATURE
```

5. 릴리스 시작 & 완료하기

```
// 릴리스 시작하기
// [BASE]: 릴리스를 시작할 commit. sha-1 해시를 선택적으로 줄 수 있음. 그 commit은 반드시 'develop' 브랜치에 있어야 함.
git flow release start RELEASE_VERSION [BASE]

// 릴리스 공유하기(upstream까지 올리기)
git flow release publish RELEASE_VERSION

// 릴리스 불러오기
git flow release track RELEASE_VERSION

// 릴리스 완료하기
git flow release finish RELEASE_VERSION

// 릴리스 배포하기
git push --tags
```

6. 핫픽스 시작 & 완료하기

```
// 핫픽스 시작하기(upstream까지 올리기)
// [BASENAME]: 핫픽스 시작점이 될 commit.
git flow hotfix start VERSION [BASENAME]

// 핫픽스 완료하기
git flow hotfix finish VERSION
```

<참고 자료>

- git-flow 관련

  - [git-flow cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html)
  - [GitFlow AVH Edition](https://git.logikum.hu/flow)
  - [[Git] git-flow 소개, 설치 및 사용법](https://hbase.tistory.com/60)
  - [[협업] 협업을 위한 Git Flow 설정하기](https://overcome-the-limits.tistory.com/7)
  - [nvie/gitflow](https://github.com/nvie/gitflow/wiki/Windows)
  - [[Git flow] feature publishing 하기](https://yujuwon.tistory.com/entry/Git-flow-feature-publishing-%ED%95%98%EA%B8%B0)
  - [Gitflow Workflow - Bitbucket](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
  - [우린 Git-flow를 사용하고 있어요 - 우아한형제들 기술 블로그](https://techblog.woowahan.com/2553/)

- etc.
  - [2.5 Git의 기초 - 리모트 저장소](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EC%A0%80%EC%9E%A5%EC%86%8C)
