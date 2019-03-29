---
title: "[명령어] 특정 포트번호로 열린 네트워크 상태보기 및 프로세스 죽이기 "
classes: wide
categories:
  - linux
date: 2019-03-29 19:00:00 -0600
---

### 특정 포트번호 네트워크 상태보기

netstat -nap | grep 825  
825 부분은 포트번호 

---

### 특정 포트번호로 listening 하고 있는 프로세스 죽이기
kill -9 $(lsof -i:6666)  
6666 부분은 포트번호
