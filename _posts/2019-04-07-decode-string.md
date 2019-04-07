---
title: "394_Decode String"
classes: wide
categories:
  - algorithm
tags:
  - stack
date: 2019-04-07 19:00:00 -0600
---

stack을 활용하여 푸는 문제이다.  
https://leetcode.com/problems/decode-string/ 

![decode_string](https://user-images.githubusercontent.com/10937193/55677235-40f27300-591f-11e9-8a0c-534093e03c64.PNG)


여기서 포인트는 "]" 스트링 값인데,  
"]"을 발견하는 순간부터 바로이전에 나왔던 "["까지의 스트링값을 보관해야한다. 

2[bc 의 경우에,  
pop()을 할때 맨뒤의 원소가 c, b 순으로 나오게 된다.  

temp_list에 push하는 경우에는 [c,b]순으로 저장되는데 나중에 stack[::-1]을 사용해서 역순으로 바꿔도 된다.

그러나 그렇게 하지않고,  
```python
prev_str = stack.pop()  
temp_list.insert(0, prev_str)
```
위와 같이 처리를 하면, temp_list에는 [b,c]순으로 저장이 된다.  

그 뒤에는 한번더 pop()을 해주면 숫자부분이 나타난다.  
해당 숫자 부분만큼 temp_string을 push해주면 된다.  

### 만약
숫자부분이 한자리수가 아니라, 10의 단위, 100위 단위라면 위 코드는 적용되기 힘들다.  

애초에 stack에 넣을때, 숫자인 경우를 계속 저장해서 
숫자가 아닌 문자 "[" 등이 나올때 한꺼번에 저장해줘야한다.  

```python
stack         = []
digit_string  = ""
for c in s:
    if c.isdigit():
        digit_string += c
    else:
        stack.append(digit_string)
        stack.append(c)
        digit_string = ""
```
또한 한꺼번에 저장해준 다음에 해당 string을 초기화 시켜주는 과정이 필요하다.  
숫자인지 아닌지 체크하는 isdigit() 함수를 사용했다.  


```python

class Solution(object):
    def decodeString(self, s):
        """
        :type s: str
        :rtype: str
        """
        
        stack = []
        digit_string = ""
        final_string = ""
        for c in s:            
            if c == "]":
                temp_list = []
                while True:
                    prev_str = stack.pop()
                    if prev_str == "[":
                        break
                    temp_list.insert(0, prev_str ) 
                temp_string = "".join(temp_list)
                
                multiply = stack.pop()
                for i in range(int(multiply)):
                    stack.append(temp_string)
            else:          
                if c.isdigit():
                    digit_string += c
                    
                else:
                    stack.append(digit_string)
                    stack.append(c)
                    digit_string = ""
            #print("".join(stack))    
                
        left_string   = "".join(stack)
        final_string += left_string
        return final_string
            
``` 

