---
layout: post
title:  "이미지를 다루는 openCV 라이브러리 python 설치 및 오류 해결"
categories: Python
tags: python directory file
author: seriousmac
---

* content
{:toc}




## openCV 라이브러리 설치 및 사용

no module named 'cv2' 오류가 나타나는 경우! openCV 라이브러리가 설치되어있지 않다는 것!
```bash
pip install opencv-python
```

하지만 이것 하나만 설치해서 되는 분들은 거의 없을 것.
아래와 같은 것들이 더 필요하기 때문!
(다른 라이브러리의 dependecy 때문에- 설치되어 있는 경우도 있겠지만..)

ImportError: libSM.so.6: cannot open shared object file: No such file or directory
```bash
sudo apt-get install libsm6 libxrender1 libfontconfig1
sudo python3 -m pip install opencv-contrib-python
```

사용방법?
```python
import cv2
```
매우 간단!!

## 폴더 내의 파일 이름 가져오기

openCV의 간단한 예제를 만들면서 디렉토리 정보를 가져오는 방법도 함께 습득해보자.

```python
import os
path ='images'
file_list = os.listdir(path) #디렉토리 내의 파일 이름들을 가져오는 것. ls 명령어로 생각하면 될듯!
file_list.sort() #sorting이 되어있지 않아서 하는데, sorting 알고리즘이 좋지는 않다. ex) 1 10 2 3 이런 식으로 됨;;;

for file_name in file_list:
    img = cv2.imread(path + '/' + file_name)
    height, width = img.shape[:2]
    s_width = 300; #원하는 width 사이즈로 입력!
    s_height = round(height / width * s_width) #원하는 width 사이즈에 맞는 비율로 계산! (및 반올림)
    small_img = cv2.resize(img, 
                       (s_width, s_height), 
                       interpolation=cv2.INTER_CUBIC) #Bi-cubic interpolation algorithm을 이용하여 resize
    #file_name = file_name.replace('_', '_300_') #변경 후 파일 이름을 바꿀 때 사용. 
    cv2.imwrite('results/' + file_name , small_img) #폴더 생성 후 
```
