---
layout: post
title: "Git Alias 설정하기"
image: 
date: 2020-6-13
tag: [git, github, alias, git setting, 깃, 깃헙, 깃 얼라이어스]
categories: []
---
### Git Alias란?
Git을 사용하다보면 수많은 명령어를 가지고 있다는 것을 깨닫습니다. 하지만 Git을 쓰다보면 그 많은 명령어와 옵션을 익히게 되고 익숙해지게 됩니다. 이 단계에서 Alias가 필요해지게 됩니다. 자주쓰는 명령어를 간단한 단축키로 설정하여 사용할 수 있게 해주는 것이 바로 Alias입니다.

***

### Alias 추가하기
Git 관련된 설정을 하기 위해 .gitconfig 파일을 수정합니다.
```
$ vim ~/.gitconfig
```
<br>
[alias] 섹션을 추가하고 섹션의 아래에 축약하고자 하는 명령어를 추가해주면 됩니다. 
```
[alias]
    s = status -s
    ci = commit
    br = branch
    co = checkout
    l = log --reflog --graph --decorate
```
<br>
간단히 몇가지 명령어를 넣었습니다. 사용하는 명령어를 잘 넣어주면 편리하게 사용할 수 있습니다. checkout과 같은 명령어를 넣어주면 확실히 편리함을 느낄 수 있습니다.
<br>

[John Grib](https://johngrib.github.io/wiki/git-alias/)님의 블로그를 참고하면 더 다양한 명령어에 대한 이야기를 보실 수 있습니다!
