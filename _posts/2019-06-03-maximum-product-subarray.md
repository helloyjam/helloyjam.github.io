---
title: "152_Maximum Product Subarray"
classes: wide
categories:
  - algorithm
tags:
  - dynamic programming
date: 2019-06-03 19:00:00 -0600
---


<https://leetcode.com/problems/maximum-product-subarray>

Example 1:  

Input: [2,3,-2,4]  
Output: 6  
Explanation: [2,3] has the largest product 6.  

Example 2:  

Input: [-2,0,-1]  
Output: 0  
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.  


### 최대가 되는 subarray 찾기  

초기에 최대가

<img width="462" alt="스크린샷 2019-06-03 오후 8 50 04" src="https://user-images.githubusercontent.com/10937193/58800021-ceadbe80-8641-11e9-83c2-1b86ca940ecb.png">

 
```python

class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        append_arr = [ [1]*len(nums) for i in range(0, len(nums)-1, 1) ]
        
        dp = [nums] + append_arr
        
        max_value = nums[0]
        for i in range(0, len(nums), 1):
            
            for j in range(i+1, len(nums), 1):
                
                if i == 0 and i+1 < len(nums) and j-1 >= 0:
                    
                    if max_value < dp[i][j]:
                        max_value = dp[i][j]
                    
                    #print(dp[i][j-1], dp[i][j])
                    
                    dp[i+1][j] = dp[i][j-1] * dp[i][j]
                    if max_value < dp[i+1][j]:
                        max_value = dp[i+1][j]
                        
                elif i > 0 and i+1 < len(nums) and j-1 >= 0:
                    #print(i,j, dp[i][j])
                    
                    dp[i+1][j] = dp[i][j-1] * dp[0][j]
                    if max_value < dp[i+1][j]:
                        max_value = dp[i+1][j]
        for d in dp:
            print(d)
            
        return max_value
``` 
