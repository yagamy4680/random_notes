### Tools for profiling your Python / Django project

https://vinta.ws/code/tools-for-profiling-your-python-django-project.html

- [timer](https://docs.python.org/2/library/timeit.html)
- [pycallgraph](https://pycallgraph.readthedocs.org/en/latest/)
- [cProfile](https://vinta.ws/code/tools-for-profiling-your-python-django-project.html)
- [line_profiler](https://github.com/rkern/line_profiler)
- [memory_profiler](https://pypi.python.org/pypi/memory_profiler)



### Python Tutorial: Comprehensions

Comprehension and Generator Syntax

```python
[transform(datum) for datum in iterable if valid(datum)]
{transform(datum) for datum in iterable if valid(datum)}
{keyFun(key): valFun(val) for key,val in iterable.items() if valid(key,val)}
(transform(datum) for datum in iterable if valid(datum))
```

List comprehensions

```python
def listComprehension(source, transform, conditional=lambda x: True):
    """
    Canonical use of a list comprehension produces a completely generated
    list of values derived from elements in a source container
    for elements that obey a condition.
    """
    return [
            transform(element)       # An operation on an element
            for element in source    # applied to elements from the source
            if conditional(element)  # for elements meeting a condition.
           ]
```

```python
def stripped(x):
    return x.strip()

def conditional(x):
    return stripped(x)

def Test3(filename='test.txt'):
    # Three ways of doing the same thing: two cumbersome and one elegant.
    container1 = []
    with open(filename) as source:
        for line in source:
            if conditional(line.strip()):
                container1.append(line.strip())
    container2 = listComprehension(open(filename), stripped, conditional)
    container3 = [stripped(line) for line in open(filename) if conditional(line)]
    # Proof
    assert container1 == container2 == container3

Test3()
```


### Python Tips and Traps

https://www.airpair.com/python/posts/python-tips-and-traps

- The Humble `enumerate`
- A member of `set`
- collections.namedtuple
- collections.defaultdict
- Generators

#### Control Flow

```python
try:
    db.commit() # may raise exception
except Exception:
    log.warn("Failure committing transaction, rolling back")
    db.rollback()
else:
    log.info("Saved the new FOO")
finally:
    db.close()
```

We've actually added two clauses here. First, let's look at the `else`, which runs if no exception occurs. In our example, all it does is log that the transaction succeeded, but you could put more interesting actions in as needed. One potential use would be to fire off a background job or notification.

The `finally` clause is there to make it clear that the `db.close()` will always run. Looking back, we can see that all the code related to persisting our data ended up in a nice logical grouping at the same indentation level. Editing this code later, it will be easy for us to see that all these lines are tied to the `commit`.


#### Context and Control

```python
class DatabaseTransaction(object):
    def __init__(self, connection_info):
        self.conn = db_library.connect(connection_info)

    def __enter__(self):
        return self.conn
```

The `__enter__` method actually does nothing except return the database connection, which we can use inside the block to retrieve or save data. The `__init__` method is where the connection is actually made, and if it fails the block won't run at all.

Now let's define how the transaction will be finished in the `__exit__` method. This has a lot more to it, since it has to handle any exceptions thrown in the block and close out the transaction.

```python
    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type is not None:
            self.conn.rollback()

        try:
            self.conn.commit()
        except Exception:
            self.conn.rollback()
        finally:
            self.conn.close()
```

Now we can use our DatabaseTransaction as the context manager for our block of actions. Under the hood, the `__enter__` and `__exit__` methods will run and handle setting up the database connection and tear it down when we're through.

```python
# context manager
with DatabaseTransaction("fakesql://") as db:
    # retrieve data here
    # modify data here
```





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


Processing messages from a child process thorough stderr and stdout with Python
http://stackoverflow.com/questions/4805576/processing-messages-from-a-child-process-thorough-stderr-and-stdout-with-python

```python
def print_pipe(type_pipe,pipe):
    for line in iter(pipe.readline, ''):
         print "[%s] %s"%(type_pipe,line),

p = Popen(cmd, bufsize=1024,
stdin=PIPE, stdout=PIPE, stderr=PIPE, close_fds=True)

t1 = Thread(target=print_pipe, args=("stdout",p.stdout,))
t1.start()
t2 = Thread(target=print_pipe, args=("stderr",p.stderr,))
t2.start()

#optionally you can join the threads to wait till p is done. This is avoidable but it 
# really depends on the application.
t1.join()
t2.join()
```

[subprocess](http://pymotw.com/2/subprocess/) – Work with additional processes


[The ever useful and neat subprocess module](http://sharats.me/the-ever-useful-and-neat-subprocess-module.html#watching-both-stdout-and-stderr)

Merge with current environment

```python
p = Popen('command', env=dict(os.environ, my_env_prop='value'))
```

[Inside Python subprocess communication](http://znasibov.info/blog/inside_python_subprocess_communication.html)

[Subprocess Module](http://www.bogotobogo.com/python/python_subprocess_module.php)

[Non blocking reading from a subprocess output stream in Python](http://eyalarubas.com/python-subproc-nonblock.html)


### UI Framework

#### Kivy

[Kivy](http://kivy.org/#home) - Open source Python library for rapid development of applications
that make use of innovative user interfaces, such as multi-touch apps.




### Open Source Projects

- [Autobahn](https://github.com/yOPERO/Autobahn), Python Twisted WebSockets plus RPC / PubSub.

- [pyrasite](http://pyrasite.com/), Tools for injecting code into running Python processes.

- [PyFormat](http://pyformat.info/), Python has had awesome string formatters for many years but the documentation on them is far too theoretic and technical. With this site we try to show you the most common use-cases covered by the old and new style string formatting API with practical examples.


### Articles

[Raspberry Pi + Arduino + Tornado](http://niltoid.com/blog/raspberry-pi-arduino-tornado/)