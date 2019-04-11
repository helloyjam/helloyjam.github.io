---
title: "784_Letter Case Permutation"
classes: wide
categories:
  - algorithm
tags:
  - backtracking
date: 2019-04-11 19:00:00 -0600
---

알파벳으로 이루어진 글자만 백트래킹하면 되는 문제이다. 초기에 모두 소문자로 변경한 다음에 알파벳이 있는 경우만
backtracking을 해주면 해결이 가능하다.  
string의 isalpha()함수를 사용하였다.  


```python
class Solution(object):
    def letterCasePermutation(self, S):
        """
        :type S: str
        :rtype: List[str]
        """
        #test_S = list("a1b2")
        final_output = []
        def backtracking(input_string, idx, n):
            
            if idx == n:
                print("".join(input_string))
                final_output.append("".join(input_string))
            else:
                backtracking(list(input_string), idx+1, n)
                
                if input_string[idx].isalpha():
                    input_string[idx] = input_string[idx].upper()
                    backtracking(list(input_string), idx+1, n)
        
        backtracking(list(S.lower()), 0, len(list(S)))
        
        return  final_output
```  
