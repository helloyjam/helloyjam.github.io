---
title: "python에서 프로세스의 cpu 사용량과 memory 사용량 체크하기"
classes: wide
categories:
  - python
date: 2019-04-09 19:00:00 -0600
---


해당 프로세스의 CPU사용량과 메모리 점유율을 체크하는 것은 무척이나 중요하다.  

리눅스에서 top명령어로 모니터링 할 수 있지만,  
이를 기록할 필요가 있을 수도 있다.  
파이썬에서는 몇 줄 안되는 코드로 이를 확인할 수 있다.  


```python
import os
import psutil

def _check_usage_of_cpu_and_memory():
    
    pid = os.getpid()
    py  = psutil.Process(pid)
    
    cpu_usage   = os.popen("ps aux | grep " + str(pid) + " | grep -v grep | awk '{print $3}'").read()
    cpu_usage   = cpu_usage.replace("\n","")
    
    memory_usage  = round(py.memory_info()[0] /2.**30, 2)
    
    print("cpu usage\t\t:", cpu_usage, "%")
    print("memory usage\t\t:", memory_usage, "%")
    
```





