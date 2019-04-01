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



#### DFS를 사용하여 해결  

DFS는 깊이 우선 탐색이라는 뜻이다.  
한쪽 노드를 타고 가장 깊은 곳 까지 같다가 더이상 갈 노드가 없으면 돌아오는 방식이다.  
이 방식의 단점은 특정 문제의 경우 노드가 무한히 깊어져서 빠져나오지 못하게 될 수도 있다.  
또한 최단 거리를 구하는 문제의 경우 처음으로 목적지를 만나도 최단거리인지 알 수가 없다.  



스택이라는 FILO 자료형을 이용하면 DFS를 재귀없이 구현할 수 있다.

방문할 스택이 필요하다  
방문한 곳은 rooms의 갯수와 동일하게 False 방문하지 않았다고 설정해준다.  
방문할 곳의 key_list들을 tovisit스택에 추가하고  

tovisit 스택을 계속 채워나간다.  

이미 방문한 곳의 열쇠는 추가하지 않기 위해  
if visited[key] == False 조건이 필요하다

```python
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

아래는 recursive로 코드를 짠 것이다.

dfs_visit이라는 함수를 따로 만들어주고,  
처음 시작점을 지정해주고(rooms[0])  
visited라는 방문했는지 여부를 체크하는 list를 넘겨준다.  

처음에 방에 들은 키(start_key)를
탐색하면서  
방문하지 않았다면 방문하고 그 방에 들은 key list(rooms[key]를 다시 재귀적으로 자신의 함수로 호출한다.  

인풋값이 아래와 같을때,
[[1,3],[3,0,1],[2],[0]]

처음 dfs_visit(rooms, rooms[0], visited)는 다음과 같은 인자를 갖는다.

rooms     = [[1,3], [3,0,1], [2], [0]]  
rooms[0]  = [1,3]  
visited   = [True, False, False, False]  

visited[0]이 True인 것은 첫번째 방문은 처음에 열면서 [1,3]를 발견하기 때문이다.   

11번 줄에서

start_key는 [1,3]이고  
1번 방문의 키로 visited[key]를 확인한다 
그리고 방문하지 않았다면 방문했다고 체크해주고  
그 방에 놓여진 키(rooms[key])를 다시 재귀적으로 호출하여 방문한다.  

1번 방문을 열고 key list를 얻는다  
visited = [True, True, False, False]  
rooms[1]은 [3,0,1]이다  
  
다시 3번 방문을 열고 key_list를 얻는다  
visited = [ True, True, False, True]
rooms[3] = [0]이다.  

0번은 이미 방문했다 (visited[0] == False)  
백트래킹해서 다시 돌아오면  
  
[3,0,1]에서 3번 방문을 열었으니,  
0번 방문을 다시 열어본다.  

0번은 이미 방문했다 (visited[0] == False)  
백트래킹해서 다시 돌아오면  
  
[3,0,1]에서 0번 방문을 열었으니,  
1번 방문을 다시 열어본다. 
  
1번은 이미 방문했다 (visited[1] == False)  
백트래킹해서 다시 돌아간다.

그럼 맨처음에 [1,3]에서 1번 방문을 재귀적으로 호출한 것이 끝난것이다.  
3번 방문을 마찬가지로 하면  
  
최종 visited = [ True, True, False, True ] 가 된다


(방문했는지 확인하는 여부는 무한루프에 빠지지 않기 위함이다.)  
이미 방문했다면 백트래킹하게 된다.  

```python
class Solution(object):
    def canVisitAllRooms(self, rooms):
        """
        :type rooms: List[List[int]]
        :rtype: bool
        """
        
        
        def dfs_visit(rooms, start_key, visited):
            #print(start_key,"START_KEY", visited)
            for key in start_key:
                if visited[key] == False:
                    visited[key] = True
                    dfs_visit(rooms, rooms[key], visited)
            
        def dfs(rooms):
                        
            visited     = [False for _ in range(len(rooms))]
            visited[0]  = True
            dfs_visit(rooms, rooms[0], visited)
        
            #print(visited,"VISITED")
            for v in visited:
                if v == False:
                    return False
                
            return True
            
        return dfs(rooms)
```   
