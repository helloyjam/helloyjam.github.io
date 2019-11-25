---
title: "해쉬(hash) 기본"
classes: wide
categories:
  - data structure
tags:
  - hash
date: 2019-11-25 19:00:00 -0600
---

해싱은 임의의 크기를 가진 데이터를 고정된 길이의 데이터로 변환시키는 것을 의미함.  
변환시키는 함수가 <strong>해쉬함수(hash function)</strong>이다. 매핑하는 과정 자체를 해싱(hashing)이라 한다.  


해시함수는 해쉬값의 개수보다 대개 많은 키값을 해쉬값으로 변환하기 때문에  
해시함수가 서로 다른 두 개의 키에 대해 동일한 해시값을 내는 해시충돌(collision)이 발생하게 된다.  

![충돌이미지](https://upload.wikimedia.org/wikipedia/commons/thumb/5/58/Hash_table_4_1_1_0_0_1_0_LL.svg/480px-Hash_table_4_1_1_0_0_1_0_LL.svg.png)

이름을 0~15 사이의 정수값으로 매핑하는 해시 함수의 예. “John Smith”와 "Sandra Dee"라는 두 키 사이에 충돌이 존재한다.  


## 충돌 해결 방법

Chainning 방법과 Open Addressing 방법이 존재한다.  

### Chainning 방법


해시충돌 문제를 해결하기 위한 간단한 아이디어 가운데 하나는 한 버킷당 들어갈 수 있는 엔트리의 수에 제한을 두지 않음으로써  
모든 자료를 해시테이블에 담는 것입니다. 해당 버킷에 데이터가 이미 있다면 체인처럼 노드를 추가하여 다음 노드를 가리키는 방식으로  
구현(연결리스트)하기 때문에 체인이라는 말이 붙은 것 같습니다. 유연하다는 장점을 가지나 메모리 문제를 야기할 수 있습니다. 아래 그림과 같습니다.  

