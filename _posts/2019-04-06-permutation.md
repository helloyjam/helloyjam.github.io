---
title: "순열 알고리즘"
classes: wide
categories:
  - algorithm
tags:
  - backtracking
date: 2019-04-06 19:00:00 -0600
---

두 요소의 교환을 통해 순열을 생성하는 과정은 아래와같다.  
첫번째 위치에 해당하는 0번 인덱스를 기준으로 나머지 요소(0,1,2,3)들과 교환하면 0번 인덱스에 각 요소들이 위치한다.  

그 다음에 그 다음 위치인 1번 인덱스를 기준으로 교환을 하면 두번째 요소도 결정된다.  

나머지 위치에 대해서도 동일하게 진행하면 된다.

![permutation_1](https://user-images.githubusercontent.com/10937193/55664607-a71dbe00-586b-11e9-9576-91d271afd5ba.PNG)

#### 주의  
교환을 통해 요소를 저장할때,  
하위노드에서 상위노드로 복귀할때, 이전에 교환한 두 요소를 재 교환해서 이전 상태로 돌아가야한다.  

```python
class Solution:
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        
        """
        final_output = []
        def backtracking(n, r):
            if n == r:
                final_output.append(list(nums))
            for i in range(r, n, 1):
                nums[r], nums[i] = nums[i], nums[r]
                backtracking(n, r+1)                
                nums[r], nums[i] = nums[i], nums[r]
            
        backtracking(len(nums), 0)
        return final_output
``` 
