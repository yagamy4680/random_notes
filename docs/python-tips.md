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