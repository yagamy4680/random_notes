
### Python

[Python 慣用語](http://seanlin.logdown.com/posts/239883-python-idioms)

1. 前言
2. If Statements
3. 請愛用 in
4. Tuple 的妙用
5. Conditional Expressions
6. 善用 enumerate
7. 負數索引值
8. 太長怎麼辦
9. loop 可以有 else
10. Chained Comparisons
11. 避免用 mutable 預設引數
12. 用 join 生成字串
13. 請愛用 dict.get()
14. 用 property 取代 getters, setters
15. Context Managers
16. 用 _ 代表未使用的變數
17. List Comprehensions
18. Generator Expressions
19. 請愛用 BIFs
20. 避免覆蓋 BIFs
21. dict.setdefault()
22. defaultdict
23. PEP 8
24. 遵循 PEP 8 的命名規則
25. import 的順序
26. 儘量別用 from module import *
27. 儘量少用 from module import obj
28. 別用 implicit relative imports
29. Convenience Imports
30. Python 之禪

### ctypes

[Python ctypes: dereferencing a pointer to a C structure](http://tentacles666.wordpress.com/2012/01/21/python-ctypes-dereferencing-a-pointer-to-a-c/)

C codes:

```c
struct foo {
    int bar;
};

struct foo *newfoo () {...}
```

Python codes:

```python
# Same structure as C codes
class Foo (ctypes.Structure):
    _fields_ = [ ("bar", c_int) ]

mylib = cdll.LoadLibrary("xxx.so")

# Force return type as pointer of that Foo structure
mylib.newfoo.restype = POINTER(Foo)

# after invoking newfoo(), you need to use the pointer’s contents member to dereference it
p = mylib.newfoo()
p.contents.bar = 17
```

[Pass python IplImage object as simple struct to shared C library using ctypes](http://www.solutionoferror.com/python/pass-python-iplimage-object-as-simple-struct-to-shared-c-library-using-ctyp-51301.asp), the author tries but still failed. And, no one tries to help author to solve this problem, at 2014/2/19.

[把 pointer to a C function 放進 ctypes Structure](https://groups.google.com/forum/#!topic/pythontw/jMfm-ZgxpNY), contains VERY USEFUL information about using ctypes to deal with pointers!!

### Open Source Projects

[Autobahn](https://github.com/yOPERO/Autobahn), Python Twisted WebSockets plus RPC / PubSub.

[pyrasite](http://pyrasite.com/), Tools for injecting code into running Python processes.


### Articles

[Raspberry Pi + Arduino + Tornado](http://niltoid.com/blog/raspberry-pi-arduino-tornado/)