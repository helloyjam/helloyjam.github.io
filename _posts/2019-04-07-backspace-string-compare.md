---
title: "844_Backspace String Compare"
classes: wide
categories:
  - algorithm
tags:
  - stack
date: 2019-04-07 19:00:00 -0600
---

stack을 활용하면 쉽게 풀 수 있는 문제이다.  
샵(#)이 들어가면 백스페이스로 지워서 스트링을 비교하면된다.  
샵이 들어갈때는 pop을 해주고 들어가지 않을때는 append를 해주면 된다.  

```python
class Solution(object):
    def backspaceCompare(self, S, T):
        """
        :type S: str
        :type T: str
        :rtype: bool
        """
        
        def compare(input_string):
            stack = []
        
            for s in input_string:
            
                if s != "#":
                    stack.append(s)
                else:
                    if stack:
                        stack.pop()
            return "".join(stack)
        
        return compare(S) == compare(T)
```
