---
title: "github code block에 line number 추가하기"
classes: wide
categories:
  - github
date: 2019-03-29 19:00:00 -0600
---

마크다운 파일(*.md)에서 코드블럭을 작성하고 싶다면,
`(backticks, 역따옴표)를 세번 사용하는 것이다.

<pre>
```python
def hello():
    print("world")
```
</pre>  

실제로 페이지는 아래와 같이 렌더링 된다. 
```python
def hello():
    print("world")
```

실제로는 위에 line number가 안보인다. 
line number를 추가하고 싶다면 두가지 방법이 있다.  
### 첫번째 방법  
###### 코드블록 작성 시 위 아래에 해당 코드를 작성한다.

`{% highlight python linenos %}`  

def hello()  
    print("world")  
`{% endhighlight %}`


### 두번째 방법  
###### _config.yml에 아래의 코드를 추가 작성한다.
<pre>
markdown: kramdown
kramdown:
    highlighter: rouge
    syntax_highlighter_opts:
        block:
            line_numbers: true
</pre>


