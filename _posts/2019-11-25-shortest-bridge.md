---
title: "934_Shortest Bridge"
classes: wide
categories:
  - algorithm
tags:
  - bfs
date: 2019-11-25 19:00:00 -0600
---

<https://leetcode.com/problems/shortest-bridge>  

Input:[[0,1],[1,0]]  
Output: 1  

### 풀이

1. 첫번째로 섬을 찾는다.  
2. 첫번째 섬과 연결된 모든 육지(1)를 다른 섬과 구분지을 수 있는 숫자가 문자로 치환해버린다.  
3. 문자로 치환해버리면서 그 치환된 위치의 좌표를 리스트로 저장한다. (bfs 리스트)  
4. bfs리스트를 사용하여 bfs 알고리즘을 사용  
5. 다른 island에 닿으면 step number를 리턴한다.  

최종 코드는 아래와 같다.

```python
class Solution(object):
    def shortestBridge(self, A):
        """
        :type A: List[List[int]]
        :rtype: int
        """
        
        def get_first():

            for i in range(0, len(A), 1):
                
                for j in range(0, len(A[0]), 1):
                    
                    if A[i][j]:
                        return i,j
                    
        i,j = get_first()
        
        bfs = []   
        print(i,j, "i,j")
        
        def dfs(i,j):
            
            A[i][j] = -1
            bfs.append((i,j))
            for x, y in ((i-1, j), (i+1, j), (i, j-1), (i, j+1)):
                #print(x,y)
                if 0 <= x < len(A) and 0 <= y < len(A[0]) and A[x][y] == 1:
                    #print(x,y)
                    
                    dfs(x,y)
        dfs(i,j)
        #print(bfs,"BFS")
        #for d in A:
        #    print(d)
        step = 0
        while bfs:
            
            new = []
            
            for i, j in bfs:
                
                for x, y in ((i-1, j), (i+1, j), (i, j-1), (i, j+1)):
                   
                    if 0 <= x < len(A) and 0 <= y < len(A[0]):
                        #print(x,y, "X_Y", A[x][y], i, j, "step", step)
                        if A[x][y] == 1:
                            return step
                        elif not A[x][y]:
                            A[x][y] = -1
                            new.append((x, y))
            
            step += 1
            #print(new,"NEW", step)
            bfs = new
            
            
            

```
  
