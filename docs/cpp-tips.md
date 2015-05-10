### Macro Variant Parameters

```c++
#ifdef DEBUG
  #define debug_printf(str, ...)     do { printf(str, __VA_ARGS__); } while (0)
#else
  #define debug_printf(str, ...)
#endif
```

### musl libc

[musl libc](http://www.musl-libc.org/), a new standard library to power a new generation of Linux-based devices. musl is lightweight, fast, simple, free, and strives to be correct in the sense of standards-conformance and safety.

[Comparison of C/POSIX standard library implementations for Linux](http://www.etalabs.net/compare_libcs.html)
