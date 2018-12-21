---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Home
permalink: 
nav_order: 1
---

# python-komsu
파이썬 꼼수 강의 (http://www.komsu.ml)

<hr>

### 파이썬 꼼수란 무엇인가?

짧은 파이썬 코드, snippet은 가르치는 도구로 사용됩니다. 파이썬 트릭은 단순한 실례로 파이썬의 한 측면을 가르치거나 **동기를 부여**하는 예제로 사용되어 더 깊히 파고들어 **직관적인 이해**를 할 수 있도록 도와줍니다.<br/>
-in "Python Tricks The Book" written by Dan Bader

**즉, 실력자들의 코드 활용법들을 보고 감탄하며 새로운 활용법을 찾을 수 있는 직관력을 키워주는 것입니다!**

## Contents

1. [깔끔한 파이썬 코드를 위한 패턴들](python-tricks/1_Clean_Patterns.md)
    - assert문, dict의 콤마, with문

2. [파이썬을 이용한 웹 크롤러(crawler) 개발!](for-web/crawler.md)

## 예제

### ex) 딕셔너리 2개를 merge 하는 방법!
```python
x = {'a': 1, 'b': 2}
y = {'b': 3, 'c': 4}
z = {**x, **y}
```
```python
>>> z
{'a':1, 'b':3, 'c':4}
```
중첩이 되는 b가 어떻게 되는지 확인하시고 순서를 정하세요!
뒤에 있는 y로 적용됩니다!

### ex) conda install 대신 pip install 
conda install 'package_name'을 하면<br/>
'PackagesNotFoundError: The following packages are not available from current channels:'라는 error가 뜨지만, <br/>
pip install 'package_name'을 하면 error가 안나는 경우,<br/>

```bash
/anaconda/envs/'venv_name'/bin/pip install 'package_name'
```

home 디렉토리에 anaconda가 있다는 가정에, venv_anme과 package_name은 본인 환경/에 맞게 수정하면 됩니다.
conda install에 사용되는 channels가 업데이트 안되었을 때 일어나는 것으로 보입니다.
직접 pip로 설치해주면 되는데, 그 경로를 지정하고 옮기기보다는, 위와같이 **conda 환경 내의 pip를 실행해줌으로써 해결**하는 것이 가장 편리합니다.
