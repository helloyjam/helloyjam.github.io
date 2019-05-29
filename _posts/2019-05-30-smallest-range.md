---
title: "632_Smallest Range"
classes: wide
categories:
  - algorithm
tags:
  - heap
date: 2019-05-29 19:00:00 -0600
---


<https://leetcode.com/problems/smallest-range>  

Input:[[4,10,15,24,26], [0,9,12,20], [5,18,22,30]]  
Output: [20,24]  
Explanation:  
List 1: [4, 10, 15, 24,26], 24 is in range [20,24].   
List 2: [0, 9, 12, 20], 20 is in range [20,24].  
List 3: [5, 18, 22, 30], 22 is in range [20,24].  



### 최소 범위  

오름차순으로 정렬된 k개의 리스트가 주어지고, k번째 리스트에는 정수(-100000 < elements < 100000)범위의 값들이 들어 있다.  
각 리스트들 중 적어도 하나의 원소를 포함한 최소 범위를 구하는 문제이다.  
<img width="644" alt="스크린샷 2019-05-30 오전 1 19 04" src="https://user-images.githubusercontent.com/10937193/58573664-30a0a980-8279-11e9-9a42-840ce9e38f3b.png">

다음과 같은 3가지 아이디어가 필요하다.  

#### 첫번째 아이디어  
k개의 리스트를 모아두었다고 가정할때, 최소 범위를 어떻게 구할 것인지 생각해보자.  

어떤 임의의 구간이 존재한다고 할때, 맨 오른쪽 위치(최고값)을 오른쪽으로 이동시키는 편이 나을까?  
아니면, 맨 왼쪽 위치(최소값)를 오른쪽으로 이동시키는 편이 나을까?  

답은 후자이다.  

#### 두번째 아이디어

k개의 리스트에 대한 위치를 가리키는 포인터가 필요하다.  
현재 최소값이 속한 리스트(k_idx)에서 그 다음 값을 포함해서  
최소거리를 계산해 보아야하기 때문이다.  

#### 세번째 아이디어  

큐의 맨 앞이 최소값을 항상 유지하려면 어떤 자료구조를 사용해야할까?  
정답은 min heap queue이다. 최소힙큐를 사용하면 낮은 값 부터 앞으로 정렬되어있다.  
<img width="819" alt="스크린샷 2019-05-30 오전 1 19 20" src="https://user-images.githubusercontent.com/10937193/58573736-4f06a500-8279-11e9-838f-6e94d0eeced4.png">  

### 필요한 것들  

이 문제를 풀기 위해 필요한 것들은 다음과 같다.

<img width="483" alt="스크린샷 2019-05-30 오전 1 19 40" src="https://user-images.githubusercontent.com/10937193/58574410-de608800-827a-11e9-9377-5788474ccd24.png">


코드는 아래와 같다.
```python
class Solution(object):
    def smallestRange(self, nums):
        """
        :type nums: List[List[int]]
        :rtype: List[int]
        """
        import heapq
        heap    = []
        
        maximum = float("-inf")
        for idx, num_list in enumerate(nums):
            heapq.heappush(heap, (num_list[0], idx)) 
            
            if maximum  < num_list[0]:
                maximum = num_list[0]
                
        # minimum value
        minimum = heap[0][0]
        minimum_range_value = maximum - minimum
        minimum_range_coord = [minimum, maximum]
        
        #print(minimum, maximum)
        #print(minimum_range_coord)
        #print(minimum_range_value)
        
        k_pointer   = [ 0 for i in range(len(nums)) ]
        
        while True:
        
            value, k_idx = heapq.heappop(heap)
        
            k_pointer[k_idx] += 1
            p = k_pointer[k_idx] 
            
            if p >= len(nums[k_idx]):
                break
            
            heapq.heappush(heap, (nums[k_idx][p], k_idx))
            if nums[k_idx][p] > maximum:
                maximum = nums[k_idx][p]
            
            minimum = heap[0][0]
            
            if minimum_range_value > maximum - minimum:
                minimum_range_value = maximum - minimum
            
                minimum_range_coord = [minimum, maximum]
            
        return minimum_range_coord            
```
  
