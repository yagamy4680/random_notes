## Tips

### The Magical `container_of()` Macro

http://radek.io/2012/11/10/magical-container_of-macro/

```c
#define container_of(ptr, type, member) ({                      \
        const typeof( ((type *)0)->member ) *__mptr = (ptr);    \
        (type *)( (char *)__mptr - offsetof(type,member) );})
```

![](http://radek.io/assets/images/posts/container_of.png)

C's [offsetof()](https://en.wikipedia.org/wiki/Offsetof) macro is an __ANSI C__ library feature found in `stddef.h`. It evaluates to the offset (in bytes) of a given member within a struct or union type, an expression of type `size_t`. 

```c
#define offsetof(st, m) ((size_t)(&((st *)0)->m))
```

### Useful Libraries

[qlibc](http://wolkykim.github.io/qlibc/) is published under 2-clause BSD license known as Simplified BSD License.

API References:

* [qlibc Core API Reference](http://wolkykim.github.io/qlibc/doc/html/files.html)
  * Containers for Key/Value pairs
    * Tree Table --- in binary tree(left-leaning red-black tree) data structure.
    * Hash Table --- in hash-based data structure.
    * Static Hash Table --- in fixed size memory(array/mmapped/shared).
    * List Table --- in (doubly) linked-list data structure.
  * Containers for Objects
    * List --- Doubly Linked List.
    * Vector --- implements a growable array of elements.
    * Queue --- FIFO(First In First Out) implementation.
    * Stack --- LIFO(Last In First Out) implementation.
  * General utilities.
    * String --- string trimmer, modifier, replacer, case converter, pattern detectors, ...
    * I/O --- non-blcking I/O, stream reader/writer, ...
    * File --- file locking, file/directory hander, path correctors, ...
    * IPC, Semaphore Shared-memory
    * En/decoders --- Url en/decoder, Base64 en/decoder, Hex en/decoder, ...
    * Hashes --- Murmur hases, FNV hases, MD5 hashes, ...
    * Time --- time diff, time format converstion, ...

* [qLibc Extension API Reference](http://wolkykim.github.io/qlibc/doc/html/files.html)
  * Apache-style Configuration File Parser.
  * INI-style Configuration File Parser.
  * HTTP client.
  * Rotating File Logger.
  * Database(MySQL) interface.
  * [Token-Bucket](http://en.wikipedia.org/wiki/Token_bucket)


### Valgrind

![](http://valgrind.org/images/st-george_sm.png)

Valgrind is a GPL'd system for debugging and profiling Linux programs. With Valgrind's tool suite you can automatically detect many memory management and threading bugs, avoiding hours of frustrating bug-hunting, making your programs more stable. You can also perform detailed profiling to help speed up your programs.

- [Memcheck](http://valgrind.org/docs/manual/mc-manual.html): a memory error detector


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


### Articles

[C Programming Substance Guidelines](https://github.com/btrask/stronglink/blob/master/SUBSTANCE.md), a.k.a Everything You Need to Know to Write Good C Code.


### Misc

#### [qrintf](https://github.com/h2o/qrintf), sprintf accelerator

The sprintf(3) family is a great set of functions for stringifying various kinds of data. The drawback is that they are slow. In certain applications, more than 10% of CPU time is consumed by the functions. The reason why it is slow is because it parses the given format at run-time.

qrintf is a preprocessor (and a set of runtime functions) that precompiles invocations of sprintf (and snprintf) with constant format strings into specialized forms.


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


## Libraries

### [for](https://github.com/cruppstahl/for/tree/master), A very fast scalar implementation for Frame Of Reference integer compression

```c
#define LEN 100
uint32_t in[LEN] = {0};
uint8_t out[512];

// Fill |in| with numbers of your choice
for (int i = 0; i < LEN; i++)
  in[i] = i;

// Now compress; can also use for_compress_sorted() if the numbers
// are sorted. This is slightly faster.
uint32_t size = for_compress_unsorted(&in[0], &out[0], LEN);
printf("compressing %u integers (%u bytes) into %u bytes\n",
        LEN, LEN * 4, size);

// Decompress again
uint32_t decompressed[LEN];
for_uncompress(&out[0], &decompressed[0], LEN);
```
