

---

## 1. Core Grammar

```python
# ── Comments ──
# Single line
"""Multi-line string, often used as a docstring (not ignored by interpreter, 
   but conventionally used for documentation)"""

# ── Indentation ──
# Python uses whitespace (4 spaces, never tabs) to define blocks.
if True:
    print("indented block")

# ── Line continuation ──
total = (1 + 2 + 3 +
         4 + 5 + 6)   # parentheses
s = "hello " \
    "world"           # backslash (avoid when possible)

# ── Multiple statements ──
x = 1; y = 2  # semicolons work but are unpythonic
```

---

## 2. Variables & Assignment

```python
# ── Basic binding ──
x = 42              # dynamic typing
x: int = 42         # type hint (no runtime effect)

# ─- Multiple assignment ──
a, b = 1, 2
a = b = c = 0       # chained assignment

# ── Unpacking ──
a, b, *rest = [1, 2, 3, 4]   # a=1, b=2, rest=[3,4]
first, *middle, last = seq

# ── Walrus operator (:=) ──
if (n := len(data)) > 10:
    print(n)

# ── Global / Nonlocal ──
def outer():
    x = 1
    def inner():
        nonlocal x
        x = 2
```

---

## 3. Data Types & Literals

```python
# ── Numbers ──
n = 42          # int
n = 1_000_000   # underscore separators (PEP 515)
f = 3.14        # float
c = 3 + 4j      # complex
b = 0b1010      # binary
o = 0o777       # octal
h = 0xFF        # hexadecimal

# ── Strings ──
s = 'single'
s = "double"
s = """multi
line"""

# ── f-strings (3.6+) ──
name = "Py"
f"Hello {name.upper()!r}"   # !r repr(), !s str(), !a ascii()

# ── Raw & formatted ──
path = r"C:\new\folder"      # raw string
f"{value:.2f}"               # format spec inside f-string

# ── Bytes ──
data = b"binary"
data = bytes([65, 66])
data = "text".encode("utf-8")

# ── Booleans & None ──
flag = True or False
null = None

# ── Constants convention ──
MAX_SIZE = 100    # ALL_CAPS (convention only)
```

---

## 4. Operators

```python
# Arithmetic: +  -  *  /  //  %  **
# Augmented: +=  -=  *=  /=  //=  %=  **=  &=  |=  ^=  <<=  >>=

# Comparison: ==  !=  <  >  <=  >=
# Identity:   is   is not
# Membership: in   not in

# Bitwise: &  |  ^  ~  <<  >>
# Logical: and  or  not

# Chained comparisons
1 < x <= 10       # equivalent to: 1 < x and x <= 10

# Ternary (conditional expression)
status = "active" if user else "inactive"

# None-coalescing
result = maybe_none or "default"
```

---

## 5. Collections

```python
# ── List ──
lst = [1, 2, 3]
lst = list(range(5))
lst.append(4)
lst.extend([5, 6])
lst.insert(0, -1)
x = lst.pop()       # last
x = lst.pop(0)      # index
lst.remove(2)       # by value
lst.sort(reverse=True, key=len)
lst.reverse()
if 3 in lst: ...

# ── Tuple (immutable) ──
t = (1, 2, 3)
t = 1, 2, 3         # parentheses optional
single = (1,)       # trailing comma required

# ── Dictionary ──
d = {"a": 1, "b": 2}
d = dict(a=1, b=2)
d["c"] = 3
val = d.get("z", 0)
val = d.setdefault("key", [])
d.update({"e": 5})
keys = d.keys()
items = d.items()   # view objects

# ── Set ──
s = {1, 2, 3}
s = set([1, 2, 2, 2])
s.add(4)
s.discard(99)       # no error if missing
s.remove(99)        # KeyError if missing
union = s1 | s2
intersection = s1 & s2
diff = s1 - s2

# ── Frozenset ──
fs = frozenset([1, 2, 3])   # immutable, hashable
```

---

## 6. Control Flow

```python
# ── if / elif / else ──
if x < 0:
    pass
elif x == 0:
    pass
else:
    pass

# ── match / case (3.10+) ──
match point:
    case (0, 0):
        print("origin")
    case (x, 0):
        print(f"x-axis at {x}")
    case (x, y) if x == y:
        print("diagonal")
    case _:
        print("somewhere else")

# ── for ──
for i in range(5):          # 0,1,2,3,4
    continue
    break
else:                       # executes if loop didn't break
    print("completed")

for i in range(10, 0, -2):  # start, stop, step
    pass

for idx, val in enumerate(items):
    pass

for k, v in d.items():
    pass

# ── while ──
while condition:
    pass
else:
    pass                    # executes if no break
```

---

## 7. Functions

```python
# ── Definition ──
def greet(name: str, greeting: str = "Hello") -> str:
    """Return a greeting string."""
    return f"{greeting}, {name}!"

# ── Call ──
greet("World")
greet(name="World", greeting="Hi")

# ── Args ──
def flexible(*args: int, **kwargs: str) -> None:
    print(args)     # tuple
    print(kwargs)   # dict

flexible(1, 2, 3, a="x", b="y")

# ── Keyword-only ──
def kwonly(a, b, *, c, d):   # c and d MUST be keyword args
    pass

# ── Positional-only (3.8+) ──
def posonly(a, b, /, c, d): # a,b MUST be positional
    pass

# ── Mutable default trap ──
def safe(items=None):
    if items is None:
        items = []

# ── Lambda ──
square = lambda x: x ** 2
sorted(data, key=lambda x: x["age"])

# ── Closures ──
def make_multiplier(n):
    def multiplier(x):
        return x * n
    return multiplier
```

---

## 8. Classes & OOP

```python
# ── Basic class ──
class Point:
    # Class variable
    dimension = 2

    def __init__(self, x: float, y: float) -> None:
        self.x = x          # instance variable
        self._y = y           # "protected" by convention
        self.__z = 0          # name-mangled to _Point__z

    def move(self, dx: float, dy: float) -> None:
        self.x += dx

    def __repr__(self) -> str:
        return f"Point({self.x}, {self._y})"

    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Point):
            return NotImplemented
        return self.x == other.x and self._y == other._y

    @classmethod
    def origin(cls) -> "Point":
        return cls(0, 0)

    @staticmethod
    def is_valid(v) -> bool:
        return isinstance(v, (int, float))

    @property
    def magnitude(self) -> float:
        return (self.x ** 2 + self._y ** 2) ** 0.5

    @magnitude.setter
    def magnitude(self, value: float) -> None:
        ratio = value / self.magnitude
        self.x *= ratio
        self._y *= ratio

# ── Inheritance ──
class ColoredPoint(Point):
    def __init__(self, x, y, color):
        super().__init__(x, y)
        self.color = color

# ── Dataclasses ──
from dataclasses import dataclass

@dataclass(frozen=True, slots=True)
class Config:
    host: str = "localhost"
    port: int = 8080

# ── Abstract base ──
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self) -> float: ...
```

---

## 9. Comprehensions & Generators

```python
# ── List comprehension ──
squares = [x**2 for x in range(10) if x % 2 == 0]

# ── Dict comprehension ──
d = {k: v for k, v in zip(keys, values) if v is not None}

# ── Set comprehension ──
s = {len(word) for word in text.split()}

# ── Generator expression ──
gen = (x**2 for x in range(1000000))  # lazy evaluation
sum(gen)

# ── Generator function ──
def countdown(n):
    while n > 0:
        yield n
        n -= 1

# ── yield from ──
def combined():
    yield from range(3)
    yield from "abc"
```

---

## 10. Context Managers

```python
# ── with statement ──
with open("file.txt") as f:
    data = f.read()

# ── Multiple contexts ──
with open("a") as a, open("b") as b:
    pass

# ── Custom context manager ──
from contextlib import contextmanager

@contextmanager
def managed_resource():
    print("acquire")
    try:
        yield resource
    finally:
        print("release")

# ── Class-based ──
class Database:
    def __enter__(self):
        self.connect()
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.disconnect()
        return False  # don't suppress exceptions
```

---

## 11. Decorators

```python
# ── Function decorator ──
import functools
import time

def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.perf_counter() - start:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)

# ── Decorator with arguments ──
def repeat(times: int):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(times=3)
def say_hello():
    print("hello")

# ── Class decorator ──
def singleton(cls):
    instance = None
    @functools.wraps(cls)
    def get_instance(*args, **kwargs):
        nonlocal instance
        if instance is None:
            instance = cls(*args, **kwargs)
        return instance
    return get_instance
```

---

## 12. Exception Handling

```python
# ── Basic ──
try:
    risky()
except ValueError as e:
    print(f"Bad value: {e}")
except (TypeError, KeyError):
    print("Type or key error")
except Exception:           # catch-all (usually avoid)
    raise                   # re-raise
else:
    print("only if no exception")
finally:
    print("always executes")

# ─- Raise ──
raise ValueError("message")
raise                       # re-raise current exception

# ── Custom exceptions ──
class ValidationError(Exception):
    def __init__(self, field, message):
        self.field = field
        super().__init__(message)

# ── Exception groups (3.11+) ──
raise ExceptionGroup("group", [ValueError(1), TypeError(2)])

# ── except* (3.11+) ──
try:
    ...
except* ValueError as eg:
    ...
```

---

## 13. Modules & Imports

```python
# ── Absolute imports ──
import os
import os.path
from collections import defaultdict
from typing import Optional as Opt

# ── Relative imports (in packages) ──
from . import module
from ..parent import something
from .sibling import func

# ── Import mechanics ──
if __name__ == "__main__":
    main()

# ── __all__ ──
__all__ = ["public_func", "PublicClass"]

# ── Lazy imports ──
def function():
    import heavy_module  # imported only when called
```

---

## 14. Async / Concurrent

```python
import asyncio

# ── Coroutine ──
async def fetch(url: str) -> bytes:
    return b"data"

# ── Await ──
async def main():
    data = await fetch("http://example.com")

# ── Running ──
asyncio.run(main())

# ── Gathering ──
results = await asyncio.gather(
    fetch("a"),
    fetch("b"),
    return_exceptions=True
)

# ── Async context manager ──
async with aiohttp.ClientSession() as session:
    ...

# ── Async iterator ──
async def ticker(delay, to):
    for i in range(to):
        yield i
        await asyncio.sleep(delay)

async for i in ticker(1, 5):
    print(i)

# ── Async comprehension ──
results = [await fetch(u) async for u in urls]
```

---

## 15. File I/O

```python
# ── Text ──
with open("file.txt", "r", encoding="utf-8") as f:
    content = f.read()          # str
    line = f.readline()
    lines = f.readlines()       # list[str]
    for line in f:              # iterate lines
        pass

# ── Binary ──
with open("data.bin", "rb") as f:
    chunk = f.read(1024)

# ── Writing ──
with open("out.txt", "w", encoding="utf-8") as f:
    f.write("hello\n")
    f.writelines(["a\n", "b\n"])
    print("formatted", file=f)

# ── Pathlib (modern) ──
from pathlib import Path
p = Path("folder") / "file.txt"
if p.exists():
    text = p.read_text(encoding="utf-8")
```

---

## 16. Type Hints (Modern)

```python
from typing import (
    Optional, Union, Literal, Callable,
    Sequence, Mapping, Iterator, Iterable,
    Generic, TypeVar, Protocol, Final, ClassVar,
    NamedTuple, TypedDict
)

# ── Basic ──
def process(items: list[int], flag: bool | None = None) -> dict[str, int]:
    ...

# ── Generic ──
T = TypeVar("T")

def first(seq: Sequence[T]) -> T | None:
    return seq[0] if seq else None

# ── Protocol (structural subtyping) ──
class Drawable(Protocol):
    def draw(self) -> None: ...

def render(obj: Drawable) -> None:
    obj.draw()

# ── TypedDict ──
class Movie(TypedDict):
    name: str
    year: NotRequired[int]

# ── Final / ClassVar ──
PI: Final[float] = 3.14159
count: ClassVar[int] = 0
```

---

## 17. Useful Built-ins & Patterns

```python
# ── Built-in functions ──
len(), range(), enumerate(), zip(), map(), filter()
reversed(), sorted(), sum(), min(), max(), any(), all()
isinstance(), issubclass(), hasattr(), getattr(), setattr()
callable(), iter(), next(), slice()

# ── Iteration tools ──
from itertools import chain, cycle, islice, groupby, permutations

# ── Functional patterns ──
from functools import partial, reduce, lru_cache, cache, singledispatch

@cache
def fib(n: int) -> int:
    if n < 2: return n
    return fib(n - 1) + fib(n - 2)

# ── String methods ──
"hello".upper(), "hello".startswith("he"), "a,b,c".split(",")
"-".join(["a", "b"]), "  hello  ".strip(), "hello".replace("l", "L")
```

---

## 18. Special Methods (Dunder Methods)

```python
class Vector:
    def __init__(self, x, y): ...
    def __repr__(self): ...           # repr(v)
    def __str__(self): ...            # str(v)
    def __eq__(self, other): ...      # ==
    def __lt__(self, other): ...      # <
    def __hash__(self): ...           # hash(v)
    def __len__(self): ...            # len(v)
    def __getitem__(self, key): ...   # v[key]
    def __setitem__(self, key, val): ...
    def __contains__(self, item): ... # item in v
    def __iter__(self): ...           # for x in v
    def __next__(self): ...           # next(v)
    def __call__(self, *args): ...    # v()
    def __add__(self, other): ...     # +
    def __enter__ / __exit__: ...     # with v
    def __bool__(self): ...           # bool(v)
    def __int__ / __float__ / __index__: .
