---
title: "513_Find Bottom Left Tree Value"
classes: wide
categories:
  - algorithm
tags:
  - binary tree
  - bfs
date: 2019-04-03 19:00:00 -0600
---




```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def findBottomLeftValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        
        queue = []
        depth = 0
        queue.append((root, depth, "root"))
        
        terminal_left = []
        
        while queue:
            
            tovisit = queue.pop(0)
            #print(tovisit[0].val, tovisit[1], tovisit[2])            
            
            if tovisit[0].left == None and tovisit[0].right == None:
                terminal_left.append((tovisit[0].val, tovisit[1], tovisit[2]))
                    
            if tovisit[0].left:       
                queue.append((tovisit[0].left, tovisit[1]+1, "left"))
                
            if tovisit[0].right:
                queue.append((tovisit[0].right, tovisit[1]+1, "right"))
            
        if len(terminal_left) == 0:
            return root.val
        
        final_depth     = []
        list_by_depth   = []
        first_depth     = None
        for idx, h in enumerate(terminal_left):
            #print(h, idx)
            #print(list_by_depth,"LIST_BY_DEPTH")
            if idx - 1 >= 0:
                if terminal_left[idx-1][1] != h[1]:
                    final_depth.append(list_by_depth)
                    list_by_depth = []
            list_by_depth.append(h)
            if idx == len(terminal_left) -1:
                final_depth.append(list_by_depth)
        
        return final_depth[::-1][0][0][0]
        
```  

단말 노드들의 depth와 left, right 정보를 모아서 리스트로 모은다음에

depth별로 list를 만들었다.  
그 후에는 역순으로 만든뒤에 첫번째 원소를 반환하면 가장 아래쪽의 left에 있는 원소가 된다.  


---

그러나, 좀 더 쉬운 방법은

bfs를 할때에, 오른쪽에서 왼쪽으로 탐색하면 가장 마지막 아래에 탐색되는 원소를 쉽게 발견할수 있더라...

```python
class Solution(object):
    def findBottomLeftValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        
        queue = [root]
        
        
        while queue:
            
            node = queue.pop(0)
            if node.right:
                queue.append(node.right)
            if node.left:
                queue.append(node.left)
        
        return node.val
```
