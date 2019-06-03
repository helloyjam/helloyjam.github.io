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

초기에 최대가 되는 subarray를 어떻게 찾을까 고민하다가,  
윈도우 사이즈 1,  
윈도우 사이즈 2,
윈도우 사이즈 3,
윈도우 사이즈 4,  
.  
.  
.  

윈도우 사이즈 별로 subarray의 product를 dp로 계산하려했다.  
아래 그림을 보면  

row별로 윈도우 사이즈에 따라서 해당 구간의 결과값을 저장했었는데,  

183/184 test case에서 에러가 발생했다. O(n^2)이었으나 Time limit excess가 떴다.  

<img width="570" alt="스크린샷 2019-06-03 오후 8 59 40" src="https://user-images.githubusercontent.com/10937193/58800334-8cd14800-8642-11e9-8196-28eabf7de743.png">

위의 방법을 코드로 작성한 것은 아래와 같다.  

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


### 두번째 시도

중간에 최대값과 최소값 사이에 존재하는 어중간한 값은 결과에 영향을 미치지 않는 것을 발견했다.  

<img width="625" alt="스크린샷 2019-06-03 오후 9 03 14" src="https://user-images.githubusercontent.com/10937193/58800519-0ec17100-8643-11e9-8f64-135f17deea4c.png">

위와 같이 현재 위치에서 얻을 수 있는 최대값과 최소값만을 가지고도 결과값을 낼 수 있다.  

첫번째 값인 2에서는 둘다 min값과 max값이 2이고,  

두번째 값인 3에서는  
3이 최소값이고, 2와 3을 곱한 6이 최대값이다.  

두번째 값인 3의 시점에서는  
min값은 3이고,  
max값은 6이다.  

이런 방식으로 최대 최소 값을 구하면 다음과 같은 그림을 그릴 수 있다.  

<img width="1028" alt="스크린샷 2019-06-03 오후 9 03 24" src="https://user-images.githubusercontent.com/10937193/58800725-8f806d00-8643-11e9-8ef4-7d472420e5af.png">


