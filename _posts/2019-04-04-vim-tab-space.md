---
title: "vim에서 tab을 space 4개로 바꾸기"
classes: wide
categories:
  - linux
date: 2019-04-04 19:00:00 -0600
---


.vimrc에 다음 내용을 추가한다.

```
set smartindent
set tabstop=4
set expandtab
set shiftwidth=4
```

기존에 tab으로 작업했던 파일을 space로 바꾸려면

vi에서 retab 명령을 실행해준다.  
:retab  

