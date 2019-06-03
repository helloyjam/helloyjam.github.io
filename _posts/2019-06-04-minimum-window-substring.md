---
title: "152_Maximum Product Subarray"
classes: wide
categories:
  - algorithm
tags:
  - sliding window
  - string
  - hash table
  - two pointers
date: 2019-06-04 19:00:00 -0600
---


<https://leetcode.com/problems/minimum-window-substring>

Example:

Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"

### 첫번째 접근

T가 포함된 최소 substring을 S에서 찾아야 한다.  
초기 조건을 설정해야한다.  

<img width="667" alt="스크린샷 2019-06-03 오후 11 42 19" src="https://user-images.githubusercontent.com/10937193/58810760-8a2e1d00-8659-11e9-8dd4-82ef6dc7bde1.png">

dict_for_init을 사용하여  
left_pointer와 right_pointer를 설정한다.

<img width="936" alt="스크린샷 2019-06-03 오후 11 42 30" src="https://user-images.githubusercontent.com/10937193/58811466-d463ce00-865a-11e9-9c22-d88d2a1635cb.png">


