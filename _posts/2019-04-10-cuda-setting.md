---
title: "gpu를 사용하기 위한 cuda 설정하기"
classes: wide
categories:
  - tensorflow
date: 2019-04-09 19:00:00 -0600
---


`nvidia-smi`로 CUDA driver 버전을 확인한다. [링크](https://docs.nvidia.com/deploy/cuda-compatibility/index.html#binary-compatibility)
`cat /proc/driver/nvidia/version`도 가능하다.  

예를 들어, 드라이버 버전이 384.81라고 가정하면, CUDA Toolkit의 버전은 9.0을 깔아야한다.  
혹시 9.2 버전을 깔게되면 아래와 같은 에러가 뜰 수 있다.

```text
tensorflow.python.framework.errors_impl.InternalError: cudaGetDevice() failed. Status: CUDA driver version is insufficient for CUDA runtime version
```

CUDA Toolkit의 로컬 설치 [링크](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#faq1)  
첫번째 방법은 Runfile installation이 있다. (sudo가 필요없음)  

```text
$ ./runfile.run --silent \
                --toolkit --toolkitpath=/my/new/toolkit \
                --samples --samplespath=/my/new/samples
```

위와 같은 명령어로 설치 후에 `nvcc --version` 또는 `cat /usr/local/cuda/version.txt` 로 버전을 확인해보자.  


cuDNN 라이브러리 설치 [링크](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html)  

cuDNN 패키지를 다운로드하고 압축을 푼다.  
`tar -xzvf cudnn-툴킷버전-linux-x64-버전.tgz`  

다음과 같은 파일들을 CUDA Toolkit directory에 복사한다.  
`$ cp cuda/include/cudnn.h /usr/local/cuda/include`  
`$ cp cuda/lib64/libcudnn* /usr/local/cuda/lib64`  
`$ chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*`  

cuDNN version을 확인하려면 다음과 같은 명령어를 사용하면된다.  
`cat $HOME/usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2`  

마지막으로 환경변수를 설정해주면 된다.
```text
export PATH="$HOME/usr/local/cuda/bin:$PATH"  
export CUDA_HOME=$HOME/usr/local/cuda
export LD_LIBRARY_PATH="$HOME/usr/local/cuda/lib64:$HOME/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH"
```
