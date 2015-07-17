### Macro Variant Parameters

```c++
#ifdef DEBUG
  #define debug_printf(str, ...)     do { printf(str, __VA_ARGS__); } while (0)
#else
  #define debug_printf(str, ...)
#endif
```

[軟體中的黑盒子 (介紹 va_lsit, va_start, va_end... 等)](http://www.dotblogs.com.tw/simplecestlavie/archive/2013/01/02/86637.aspx)

```c
#define MyLog(x, ...)  logInfo("[MyApplication][%s] "x, __FUNCTION__, ##__VA_ARGS__);
#define MyLog(x, ...)  logWarning("[MyApplication][$s][Warning] "x, __FUNCTION__, ##__VA_ARGS__);
#define MyLog(x, ...)  logError("[MyApplication][$s][Error] "x, __FUNCTION__, ##__VA_ARGS__);
```


**Simple Popen2 Implementation**

[http://dzone.com/snippets/simple-popen2-implementation](http://dzone.com/snippets/simple-popen2-implementation)

popen2 implementation. This is similar to popen, but allows for bidirectional communication with the application being executed.



### musl libc

[musl libc](http://www.musl-libc.org/), a new standard library to power a new generation of Linux-based devices. musl is lightweight, fast, simple, free, and strives to be correct in the sense of standards-conformance and safety.

[Comparison of C/POSIX standard library implementations for Linux](http://www.etalabs.net/compare_libcs.html)
	