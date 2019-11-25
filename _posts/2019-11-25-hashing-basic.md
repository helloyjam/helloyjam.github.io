---
title: "해싱(hashing) 기본"
classes: wide
categories:
  - data structure
tags:
  - hash
date: 2019-11-25 19:01:00 -0600
---

해싱은 임의의 크기를 가진 데이터를 고정된 길이의 데이터로 변환시키는 것을 의미함.  
변환시키는 함수가 <strong>해쉬함수(hash function)</strong>이다. 매핑하는 과정 자체를 해싱(hashing)이라 한다.  

## Direct Address Table (직접 주소 테이블)

![직접 주소테이블](https://www.geeksforgeeks.org/wp-content/uploads/hmap.png)

## Hash Table

해시함수는 해쉬값의 개수보다 대개 많은 키값을 해쉬값으로 변환하기 때문에  
해시함수가 서로 다른 두 개의 키에 대해 동일한 해시값을 내는 해시충돌(collision)이 발생하게 된다.  

![충돌이미지](https://upload.wikimedia.org/wikipedia/commons/thumb/5/58/Hash_table_4_1_1_0_0_1_0_LL.svg/480px-Hash_table_4_1_1_0_0_1_0_LL.svg.png)

이름을 0~15 사이의 정수값으로 매핑하는 해시 함수의 예. “John Smith”와 "Sandra Dee"라는 두 키 사이에 충돌이 존재한다.  


## 충돌 해결 방법

Chaining 방법과 Open Addressing 방법이 존재한다.  

### Chaining 방법 - 충돌을 허용하되 최소화하는 방법


한 버킷당 들어갈 수 있는 엔트리의 수에 제한을 두지 않음으로써 모든 자료를 해시테이블에 담는 방식이다.  
해당 버킷에 데이터가 이미 있다면 체인처럼 노드를 추가하여 다음 노드를 가리키는 방식으로 구현(연결리스트)하기 때문에 체이닝이다.  
유연하다는 장점을 가지나 메모리 문제를 야기할 수 있다.  

때문에, 최초의 위치를 탐색하는 해쉬 과정은 제외하고 모든 탐색, 삽입, 삭제 과정은 연결리스트와 유사한 방식으로 진행된다.  

![충돌해결 이미지](https://i.imgur.com/7PTT8dT.png)

### Open Addressing 방법 
