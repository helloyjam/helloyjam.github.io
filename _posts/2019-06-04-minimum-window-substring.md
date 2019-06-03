---
title: "76_Minimum Window Subtring"
classes: wide
categories:
  - algorithm
tags:
  - sliding window
  - string
  - hash table
  - two pointers
date: 2019-06-04 19:00:00 -0600
---


<https://leetcode.com/problems/minimum-window-substring>

Example:

Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"

### 첫번째 접근

T가 포함된 최소 substring을 S에서 찾아야 한다.  
초기 조건을 설정해야한다.  

<img width="667" alt="스크린샷 2019-06-03 오후 11 42 19" src="https://user-images.githubusercontent.com/10937193/58810760-8a2e1d00-8659-11e9-8dd4-82ef6dc7bde1.png">

dict_for_init을 사용하여  
left_pointer와 right_pointer를 설정한다.

<img width="936" alt="스크린샷 2019-06-03 오후 11 42 30" src="https://user-images.githubusercontent.com/10937193/58811466-d463ce00-865a-11e9-9c22-d88d2a1635cb.png">

현재 포인터 사이의 T를 충족하는 문자의 카운트 사전인 current_dict와,  
T를 만족하는 사전인 satisfied_dict가 필요하다.  

예를 들면, 
S = "ADOBBECODEBAC"  
T = "ABC" 
라고 했을때, 

current_dict    = {'A':1, 'B':2, 'C':1} 이고  
satisfied_dict  = {'A':1, 'B':1, 'C':1} 이다.  

둘 사이에 차이가 있다는 점에 유의하자.  

<img width="1023" alt="스크린샷 2019-06-03 오후 11 42 45" src="https://user-images.githubusercontent.com/10937193/58811757-5bb14180-865b-11e9-98e1-6fa532abd73b.png">

left_pointer,  
right_pointer,  
current_dict,  
satisfied_dict,  

위 4가지 값이 있어야 윈도우 슬라이딩이 가능하고,  
left_pointer와 right_pointer를 사용하여  
현재 위치의 minimum_length와 그에 대응하는 문자열을 알아낼 수 있다.  

<img width="715" alt="스크린샷 2019-06-03 오후 11 43 23" src="https://user-images.githubusercontent.com/10937193/58812193-4688e280-865c-11e9-9328-80e148c4bfeb.png">

초기 상태는 다음과 같다.

<img width="485" alt="스크린샷 2019-06-03 오후 11 43 36" src="https://user-images.githubusercontent.com/10937193/58812329-949de600-865c-11e9-83a5-2672c21fb4d2.png">

위 초기 상태에서, left_pointer를 오른쪽으로 움직여도 되는지 여부가 매우 중요한 분기이다.  
오른쪽으로 움직여도 satisfied_dict를 충족한다면, 움직여도된다.  

그러나 충족하지 않는다면 right_pointer를 오른쪽으로 옮긴다.  

그 후에 pointer가 이동한 상태에서의 current_dict를 업데이트 해주고  
해당 포인터의 최소길이와 문자열도 업데이트 해준다.  

언제까지 지속하는지 여부는 pointer가 len(S)의 길이를 초과할때까지이다.

<img width="606" alt="스크린샷 2019-06-03 오후 11 43 45" src="https://user-images.githubusercontent.com/10937193/58812346-a1bad500-865c-11e9-82c6-e55afdd0f572.png">

아래는 포인터를 조건에 맞게 오른쪽으로 움직이는 것을 설명한 그림이다.  
첫번째 상황은 초기조건이다.  

두번째 상황은 right_pointer를 새로운 A를 만날때까지 오른쪽으로 움직여준 상황이다.  

세번째 상황은 새로운 A를 만났으니  
앞에있던 left_pointer를 오른쪽으로 C까지 움직였다.(조건에 충족되는 선에서)  
CODEBA에서 ODEBA로 left_pointer를 못 움직이는 이유는  
satisfied_dict 조건에 충족하지 않기 때문이다.  

ODEBA 는 {'A':1, 'B':1} 이기 때문...  

satisfied_dict는 {'A':1, 'B':1, 'C':1}이다.

<img width="403" alt="스크린샷 2019-06-03 오후 11 44 08" src="https://user-images.githubusercontent.com/10937193/58812496-f9f1d700-865c-11e9-9628-e02e7f994adc.png">

네번째 상황은 다시 right_pointer를 마지막 단어인 C를 만날때까지 오른쪽으로 옮겼다.

이때 current_dict는 {'A':1, 'B':1, 'C':2}이다.
current_dict가 satisfied_dict를 충족하므로

left_pointer를 B를 만날때까지 오른쪽으로 옮긴다.

그 상황이 마지막 다섯번째를 뜻한다.


코드는 아래와 같다.



```python

class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        dict_for_init   = {}
        check_satisfied = {}
        
        for a in t:
            if a in check_satisfied:
                check_satisfied[a]  += 1
                dict_for_init[a]    += 1
            else:
                check_satisfied[a]  = 1
                dict_for_init[a]    = 1
    
        left_pointer    = None
        right_pointer   = None
        
        for idx, c in enumerate(s):            
            if c in dict_for_init:                
                if left_pointer == None:
                    left_pointer = idx
                    dict_for_init[c] -= 1                
                else:
                    dict_for_init[c] -= 1
            
                if dict_for_init[c] == 0:
                    del dict_for_init[c]
            
            if len(dict_for_init) == 0:
                
                if right_pointer == None:
                    right_pointer = idx
        
        #print(left_pointer, right_pointer)
        
        #### start...
        if left_pointer == None or right_pointer == None:
            return ""
        #final_output = ""
        minimum_length  = right_pointer - left_pointer
        final_output    = s[left_pointer:right_pointer+1]
        
        print("START_WINDOW",final_output)
        
        
        def _initial_dict(initial_string, check_dict):
            
            current_dict = {}
            for s in initial_string:

                if s in check_dict:
                    
                    if s not in current_dict:
                        current_dict[s] = 1
                        
                    else:
                        current_dict[s] +=1
                        
            return current_dict
                
                
            
        current_dict = _initial_dict(final_output, check_satisfied)
        print(current_dict,"CURRENT_DICT")
        print(minimum_length, "MINUMUM_length")

        def _move_left_pointer(left_pointer, right_pointer, s, current_dict):
            
            alphabet = s[left_pointer-1]
            
            #print(alphabet, s[left_pointer:right_pointer+1], current_dict)
            if alphabet in check_satisfied:
                
                if current_dict[alphabet]-1 < check_satisfied[alphabet]:
                    
                    # cannot remove alphabet.
                    return False
                else:
                    current_dict[alphabet] -= 1                    
            return True           
        
        while True:
            
            if left_pointer + 1 < len(s):
                
                if _move_left_pointer(left_pointer+1, right_pointer, s, current_dict):
                    
                    # True
                    left_pointer += 1
                    
                    if minimum_length > right_pointer - left_pointer:
                        minimum_length  = right_pointer - left_pointer
                        final_output    = s[left_pointer:right_pointer+1]
                        #print(final_output,"------------", left_pointer, right_pointer)
                    
                else:
                    
                    if right_pointer + 1 < len(s):
                        
                        right_pointer += 1
                        
                        alphabet = s[right_pointer]
                        
                        if alphabet in check_satisfied:
                            
                            if alphabet in current_dict:
                                
                                current_dict[alphabet] += 1
                
                        #_move_right_pointer(left_pointer, right_pointer+1, s, current_dict)
                    else:
                        break
            else:
                break
            print(left_pointer, right_pointer, len(s), s[left_pointer:right_pointer+1])
            #if right_pointer > len(s)-1:
            #    break
            #print()    
            #print(final_output)       
                
                
            #break
        return final_output
        

        
        
```
