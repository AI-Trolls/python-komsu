---
layout: post
title:  "파이썬을 이용한 배치 제네레이터 정리 코드 (batch generator / yield)"
categories: Python
tags: python batch generator
author: seriousmac
---

## batch generator

```python
def batch_generator(input_list, batch_size):
    start = 0
    while True:
        if len(input_list) >= start + batch_size:
            yield input_list[start:start + batch_size]
        else:
            yield input_list[start:]
            break
        start += batch_size


if __name__ == '__main__':
    image_list = []
    gen = batch_generator(image_list, 64)
    for images in gen:
        model.predict(images)
```

- generator 함수는 한 번에 한 번의 yield의 위치까지 실행하는 것이라 보면 됨!
- 따라서 for문에서 gen(generator)를 호출 할 때마다, yeild까지 실행되고 return함!
- 파일이나 데이터를 전체를 다 읽어서 자르지 않고 부분을 잘라 처리할 수 있도록 함
- 메모리 효용성 증대
- 속도는 뭐 그닥 달라질 것 없을 듯
