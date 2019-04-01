---
title: "841_Keys and Rooms"
classes: wide
categories:
  - algorithm
tags:
  - dfs
  - recursive
date: 2019-04-01 19:00:00 -0600
---

https://leetcode.com/problems/keys-and-rooms/



DFS를 사용하여 해결  

스택이라는 FILO 자료형을 이용하면 DFS를 재귀없이 구현할 수 있다.

방문할 스택이 필요하다  
방문한 곳은 rooms의 갯수와 동일하게 False 방문하지 않았다고 설정해준다.  
방문할 곳의 key_list들을 tovisit스택에 추가하고  

tovisit 스택을 계속 채워나간다.  

이미 방문한 곳의 열쇠는 추가하지 않기 위해  
if visited[key] == False 조건이 필요하다

```
class Solution(object):
    def canVisitAllRooms(self, rooms):
        """
        :type rooms: List[List[int]]
        :rtype: bool
        """
        def dfs(rooms, start):
            
            print(rooms, start)
            tovisit     = [ start ]
            visited     = [ False for _ in range(len(rooms)) ]
            visited[0]  = True

            
            while tovisit:
                key_list = tovisit.pop()    
                #print(key_list,"KEY_LIST")
                
                for key in key_list:
                    
                    if visited[key] == False:
                        visited[key] = True
                        tovisit.append(rooms[key])
                        
            for v in visited:
                if v == False:
                    return False
                
            return True               
                    
                
        return dfs(rooms, rooms[0])
```
