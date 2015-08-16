## Tips

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


### StackTrace

[google-breakpad](https://code.google.com/p/google-breakpad/wiki/GettingStartedWithBreakpad)

Breakpad is a library and tool suite that allows you to distribute an application to users with compiler-provided debugging information removed, record crashes in compact "minidump" files, send them back to your server, and produce C and C++ stack traces from these minidumps.

![](http://google-breakpad.googlecode.com/svn/wiki/breakpad.png)





## LibC

### musl libc

[musl libc](http://www.musl-libc.org/), a new standard library to power a new generation of Linux-based devices. musl is lightweight, fast, simple, free, and strives to be correct in the sense of standards-conformance and safety.

[Comparison of C/POSIX standard library implementations for Linux](http://www.etalabs.net/compare_libcs.html)


### Malloc implementations

- [C-Malloc-Implementation](https://github.com/tomelm/C-Malloc-Implementation), An implementation of the malloc library in C for a homework assignment
- [dlmalloc](http://gee.cs.oswego.edu/pub/misc/malloc.c), malloc/free/realloc written by
  Doug Lea and released to the public domain (Version 2.8.6 Wed Aug 29 06:57:58 2012  Doug Lea), the design is explained [here](http://g.oswego.edu/dl/html/malloc.html)
- [Tlsf](http://tlsf.baisoku.org/)
  - O(1) cost for malloc, free, realloc, memalign, which could be important for real-time systems,
  - Extremely low overhead per allocation (4 bytes),
  - Low overhead per pool (~3kB),
  - Low fragmentation,
  - And it compiles to only a few kB of code and data, But if you have only a few KB to work with, this RAM memory overhead is still too much…
- [Memory manager](https://github.com/eliben/code-for-blog/tree/master/2008/memmgr), provides memory from a fixed pool that is allocated statically at link-time. Useful for embedded systems, where dynamic allocation is not always supported.

http://g.oswego.edu/dl/html/malloc.html

A good article [Open source implementations of malloc](http://www.rtos.be/2014/05/open-source-implementations-of-malloc/) shall be read.


## System

### Simple Popen2 Implementation

[http://dzone.com/snippets/simple-popen2-implementation](http://dzone.com/snippets/simple-popen2-implementation)

popen2 implementation. This is similar to popen, but allows for bidirectional communication with the application being executed.
