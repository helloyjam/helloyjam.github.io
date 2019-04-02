---
title: "993_Cousins in Binary Tree"
classes: wide
categories:
  - algorithm
tags:
  - binary tree
  - bfs
date: 2019-04-02 19:00:00 -0600
---

https://leetcode.com/problems/cousins-in-binary-tree/

binary tree를 순회하면서  
dictionary에 해당 노드의 depth와 parent node를 표시해주었다.  

그 뒤 x,y값의 깊이가 같은지 확인하고 부모노드가 같은지 확인하면 된다.

```
class Solution(object):
    def isCousins(self, root, x, y):
        """
        :type root: TreeNode
        :type x: int
        :type y: int
        :rtype: bool
        """
        dictionary = {}
        dictionary[root.val] = (0, root)
        def preorderTraverse(root, depth):
            
            if root:
                #print(root.val)
                if root.left:
                    dictionary[root.left.val] = (depth, root)
                if root.right:
                    dictionary[root.right.val] = (depth, root)
                
                
                preorderTraverse(root.left, depth+1)                
                preorderTraverse(root.right, depth+1)
        
        
        preorderTraverse(root, 1)
        
        if dictionary[x][0] == dictionary[y][0] and dictionary[x][1] != dictionary[y][1]:
            return True
        
        else:
            return False
```
