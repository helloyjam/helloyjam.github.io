---
title: "shuffle()에서 buffer_size의 중요성"
classes: wide
categories:
  - tensorflow
date: 2019-04-05 19:00:00 -0600
---

tf.data.Dataset은 대량의 데이터를 표현할 수 있는 API이다.  
(tensorflow 공식사이트에서는, 잠재적으로 큰 요소 집합을 나타낸다고 말한다.)  
Dataset은 input pipeline을 표현하는데 사용될 수 있다.  

그 중에서 오늘 기록하고 싶은 것은  
input pipeline을 생성하는데 training 데이터를 랜덤하게 셔플하는 기능에 대한 것이다.  

shuffle()은 해당 dataset의 원소를 랜덤하게 셔플해준다고 하는데,  
이 함수의 parameter 중 하나는 buffer_size가 있다.  

예를 들어,  
고양이 이미지를 학습중이며 데이터가 다음과 같은 방식으로 가정해보자 ( 각 카테고리에 10000개의 이미지가 있음 )  

```
train/  
  cat/
    filename_00001.jpg
    filename_00002.jpg
    ...
  not_cat/
    filename_10001.jpg
    filename_10002.jpg
    ...
```

tf.data를 사용하여 데이터를 입력하는 스탠다드한 방법은  
파일 이름 목록과 그에 대응되는 라벨 목록을 가져오고  
tf.data.Dataset.from_tensor_slices()를 사용하여 dataset을 만드는 것이다.  

```
filenames = ["filename_00001.jpg", "filename_00002.jpg", ... ,
             "filename_10001.jpg", "filename_10002.jpg", ... ]
labels    = [1, 1, ... , 0, 0, ... ] # 1은 고양이, 0은 고양이가 아닌것

dataset   = tf.data.Dataset.from_tensor_slices((filenames, labels))
dataset   = dataset.shuffle(buffer_size=1000) # 1000이 충분히 옳은가?  
dataset   = dataset.batch(100)
```

위 코드의 큰 문제는 dataset이 올바른 방법으로 실제로 섞이지 않는다는 것이다.  
고양이 이미지만 dataset에서 보게 될 것이다.  
이는 training의 성능을 낮추게 될 것이 분명하다.  

training을 시작할때, dataset은 처음 1000개의 filenames를 buffer에 가져올것이고,  
그런 다음에 1000개 중에 랜덤하게 100개를 pick할 것이다.  

모든 첫 1000개의 이미지는 고양이 이미지이기 때문에 오직 고양이 이미지만 pick하게 되는것이다.  

그렇다면 buffer_size는 몇으로 설정해주어야할까?  
buffer_size를 20000이상으로 설정하거나,  
사전에 파일이름과 라벨을 shuffle해서 dataset을 만들어야 한다.  
(사전에 shuffle을 할 경우에도 물론 파일이름과 라벨 순서는 동일하도록 해야한다.)  

또한, 모든 파일이름과 라벨을 메모리에 저장하는것은 큰 문제가 아니므로
buffer_size=len(filenames)을 사용할 수 있다.  

이미지 읽고 프로세싱하고 배치작업등의 무거운 작업을 하기전에 tf.data.Dataset.shuffle()을 호출해야하는것을 명심해야한다.  




