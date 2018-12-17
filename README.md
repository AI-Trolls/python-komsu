# python-komsu
파이썬 꼼수 강의

<hr>

### 파이썬 꼼수란 무엇인가?

짧은 파이썬 코드, snippet은 가르치는 도구로 사용됩니다. 파이썬 트릭은 단순한 실례로 파이썬의 한 측면을 가르치거나 **동기를 부여**하는 예제로 사용되어 더 깊히 파고들어 **직관적인 이해**를 할 수 있도록 도와줍니다.<br/>
-in "Python Tricks The Book" written by Dan Bader

**즉, 실력자들의 코드 활용법들을 보고 감탄하며 새로운 활용법을 찾을 수 있는 직관력을 키워주는 것입니다!**

## Contents

1. [깔끔한 파이썬 코드를 위한 패턴들](python-tricks/1_Clean_Patterns.md)
    - assert문, dict의 콤마, with문

## 예제

ex) 딕셔너리 2개를 merge 하는 방법!
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
