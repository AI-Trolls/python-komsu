# 1. 깔끔한 파이썬 코드를 위한 패턴들

때로는 **태생적으로** 도움이되는 언어의 기능들이 덜 주목받기도 합니다. 즉, 파이썬이 가진 프로그래밍 언어의 **특성**을 잘 활용해야 합니다.

## 1.1. 파이썬에서의 Assert (Assertion)

assert는 문은 조건을 테스트하는 디버깅을 도와줍니다.
예를들어 파이썬으로 온라인 상점을 개발한다고 가정합시다. 시스템에 할인 쿠폰 기능을 추가하려고 한다고 할 때, apply_discount 함수를 정의하는 상황입니다.

```python
def apply_discount(product, discount):
  price = int(product['price'] * (1.0 - discount))
  assert 0 <= price <= product['price']
  return price
```

assert문이 있는 부분을 주의해서 보세요. 할인된 가격은 함수에 의해서 계산되고 0원 보다 낮을 수 없으며, 원가보다 높을 수 없는 것이 보장됩니다. 바로 assertion에 의해서요!!

```python
>>> games = {'name': 'Komsu game', 'price': 19900}
>>> print( apply_discount(games, 0.25) )
```

실제로 그 결과를 보기 위해서는 위와 같이 하시면 됩니다. 할인이 잘 되는 결과를 보실 수 있구요, 아래처럼 200% 할인을 하게 되면, 금액을 오히려 돌려줘야하는 말도 안되는 상황이 생깁니다.

```python
>>> print( apply_discount(games, 2.0) )
```

하지만 결과는? AssertionError 메세지를 만나게되죠. 왜냐하면 200%의 할인은 assertion 조건을 위반하기 때문입니다!
따라서 이러한 assert를 잘 활용하는 것이 디버깅을 더욱 효과적으로, 빠르게 하는데 도움이 됩니다.

Python 문서에 나와있는 Assert 문법을 살펴볼까요,

```python
assert_stmt ::=  "assert" expression ["," expression]
```

위는 simple form이구요, 아래와 동등한 기능을 합니다.

```python
if __debug__:
    if not expression: raise AssertionError
````
좀더 확장된 형태로 assert expression1, expression2 하는 것은 아래와 같습니다. expression2는 AssertionError메세지와 함께 traceback에 보여집니다.

```python
if __debug__:
    if not expression1: raise AssertionError(expression2)
```

가장 중요한 주의사항으로, assert에 tuple을 넣을 경우 항상 true로 처리되어버립니다. 

```python
assert(1==2,' This should fail')
```

아래와 같은 syntax warning이 뜹니다.

```python
<>:1: SyntaxWarning: assertion is always true, perhaps remove parentheses?
```

## 1.2. 자기만 만족하는 쉼표 배치

```python
names = ['komsu', 'python', 'tricks']
```

위의 코드는 error가 났을 경우 혹은 Git diff를 사용할 경우, 여러 변화를 한 line으로 highlighting 할 수 있습니다. 따라서 아래와 같이 작성하는 것이 더 분명하게 표현할 수 있습니다.

```python
names = [
  'komsu',
  'python',
  'tricks'
]
```

여기서 만약에, 쉼표를 빼먹으면 어떻게 될까요?

```python
names = [
  'komsu',
  'python',
  'tricks'
  'new'
]
```

Dilbert와 Jane 사이에 쉼표를 실수로 빼먹었다고 가정해봅시다. 그 결과는, 'tricksnew' 하나로 합쳐집니다. 이것은 파이썬에서 string을 사용할 때 나타나는 특성입니다.

## 1.3. context managers(컨텍스트 관리자)와 with문

python에서 with문은 때로 애매한 기능으로 취급받습니다. 하지만 알고보면 매우 깔끔하고 가독성있는 코드를 만드는데 유용합니다.
with문은 공통 자원 관리 패턴을 단순화시키는데 도움을 줍니다. 그 기능성을 추상화하고, 그것을 배제하고 재사용되는 것을 허가하는데 도움이 됩니다.

```python
with open('komsu.txt', 'w') as f:
  f.write('hello, universe')
```

이렇게 코드를 사용하면 파일 기술자(file descripters)가 열려있는 것을 보증하고, with문을 떠날 때 자동적으로 닫습니다. 내부적으로 위의 코드는 아래와 같습니다.

```python
f = open('komsu.txt', 'w')
try:
  f.write('hello, universe')
finally:
  f.close()
```

또 다른 with문 활용법은 python standard library인 threading.Lock Class를 활용할 때입니다.

```python
some_lock.acquire()
try:
  # Do something...
finally:
  some_lock.release()
``

대신에 아래와 같이 활용...

```python
with some_lock:
  # Do Something...
```

지금까지 설명한 with문은 그렇게 마법같은 일이 벌어지진 않았죠. 하지만, context managers에서는 다를겁니다!

context manager(컨텍스트 관리자): 객체(ojbect)가 with문을 사용하기 위해 따라야하는 protocol (혹은 interface)

사용하는 방법은 간단합니다. context manager의 기능을 할 객체에 ```__enter__```와 ```__exit__``` 함수를 추가하면 됩니다.

python은 자원 관리 사이클에서 적절한 때에 두 함수를 호출할 것입니다.

```python
class ManagedFile:
  def __init__(self, name):
    self.name = name
  def __enter__(self):
    self.file = open(self.name, 'w')
    return self.file
  def __exit__(self, exc_type, exc_val, exc_tb):
    if self.file:
      self.file.close()
```

위와 같이 class를 선언하면 with문을 사용할 수 있게 됩니다.

```python
with ManagedFile('komsu.txt') as f:
  f.write('hello, universe')
  f.write('bye now')
```

with문으로 들어갈 때, ```__enter__``` 함수가 호출되고, 그 때가 바로 자원을 얻을 때입니다. context를 떠날 때, 자원을 해제해주기 위해 파이썬은 ```__exit___```을 호출합니다.

class-based context manager를 사용해야만 with문을 사용할 수 있는 것은 아닙니다. 파이썬 스탠다드 라이브러리에서 제공하는 contextlib 모듈을 사용하면 좀 더 쉽게 사용할 수 있습니다.

```python
@contextmanager
def managed_file(name):
  try managed_file(name):
    f = open(name, 'w')
    yield f
  finally:
    f.close()
    
with managed_file('komsu.txt') as f:
  f.write('hello, universe')
  f.write('bye now')
```

위와 같은 방법은 managed_file()는 자원을 얻는 generator 입니다. 그 후 이것은 일시적으로 실행을 잠시 미루고, 자원을 yeild합니다. caller가 떠나고 나면 generator는 실행을 재개하고 자원을 해제하기 시작합니다. 이 방법은 generator를 사용하기 때문에, generator-based implementation으로 불립니다. 기능적으로는 동등하기 때문에, 가독적인 부분에서 선택하셔서 사용하면 됩니다.

<hr>

### reference

1. [Python Documentation 3.7.2rc1 7. Simple statements](https://docs.python.org/3/reference/simple_stmts.html?highlight=assert#grammar-token-assert-stmt)
2. Python Tricks The Book: A Buffet of Awesome Python Features, Dan Bader.
