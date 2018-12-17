# 1. 깔끔한 파이썬을 위한 패턴들

## 1.1. 파이썬에서의 Assert (Assertion)

때로는 **태생적으로** 도움이되는 언어의 기능들이 덜 주목받기도 합니다. 즉, 파이썬이 가진 프로그래밍 언어의 **특성**을 잘 활용해야 합니다.
파이썬의 가장 핵심적인 특징 중 하나는 assert문입니다. assert는 문은 조건을 테스트하는 디버깅을 도와줍니다.

예를들어 파이썬으로 온라인 상점을 개발한다고 가정합시다. 시스템에 할인 쿠폰 기능을 추가하려고 한다고 할 때, apply_discount 함수를 정의하는 상황입니다.

```python
def apply_discount(product, discount):
  price = int(product['price'] * (1.0 - discount))
  assert 0 <= price <= product['price']
  return price
```

assert문이 있는 부분을 주의해서 보세요. 할인된 가격은 함수에 의해서 계산되고 0원 보다 낮을 수 없으며, 원가보다 높을 수 없는 것이 보장됩니다. 바로 assertion에 의해서요!!

```python
>>> shoes = {'name': 'Komsu shoes', 'price': 19900}
>>> apply_discount(shoes, 0.25)
```

실제로 그 결과를 보기 위해서는 위와 같이 하시면 됩니다. 할인이 잘 되는 결과를 보실 수 있구요, 아래처럼 200% 할인을 하게 되면, 금액을 오히려 돌려줘야하는 말도 안되는 상황이 생깁니다.

```python
>>> apply_discount(shoes, 2.0)
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

<hr>
reference
1. Python Documentation 3.7.2rc1 7. Simple statements
   (https://docs.python.org/3/reference/simple_stmts.html?highlight=assert#grammar-token-assert-stmt)
2. Python Tricks The Book: A Buffet of Awesome Python Features, Dan Bader.
